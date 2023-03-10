#   结构体
##  1.  自定义类型
::: tip 提示
1. 在Go语言中有一些基本的数据类型，如string、整型、浮点型、布尔等数据类型， Go语言中可以使用type关键字来定义自定义类型。
2. 自定义类型是定义了一个全新的类型。我们可以基于内置的基本类型定义，也可以通过struct定义。
:::
```go
package main

import (
	"fmt"
	"reflect"
)
// 将newInt定义为int类型
type newInt int

func main() {
	var i newInt
	i = 1
	fmt.Println(i)                 // 1
	fmt.Println(reflect.TypeOf(i)) // main.newInt
}
```
::: danger 注意了
上例中的`newInt`是具有`int`特性的新类型。可以看到变量i的类型是`main.newInt`，这表示`main`包下定义的`newInt`类型。
:::
##  2.  类型别名
::: tip 提示
类型别名规定：TypeAlias只是Type的别名，本质上TypeAlias与Type是同一个类型。就像一个孩子小时候有小名、乳名，上学后用学名，英语老师又会给他起英文名，但这些名字都指的是他本人。
:::
<font color="blue"><b>例如:</b></font>
```shell
type newInt = int
```
1. type 关键字
2. newInt 别名
3. 类型名称
```go
package main

import (
	"fmt"
	"reflect"
)

// 给int取一个别名newInt
type newInt = int

func main() {
	var i newInt
	i = 1
	fmt.Println(i)                 // 1
	fmt.Println(reflect.TypeOf(i)) // int
}
```
##  3. 类型定义和类型别名的区别
类型别名与类型定义表面上看`只有一个等号的差异`，我们通过下面的这段代码来理解它们之间的区别。
```go
package main

import "fmt"

//类型定义
type NewInt int

//类型别名
type MyInt = int

func main() {
	var a NewInt
	var b MyInt

	fmt.Printf("type of a:%T\n", a) //type of a:main.NewInt
	fmt.Printf("type of b:%T\n", b) //type of b:int
}
```
:::danger 注意了
1. 结果显示a的类型是main.NewInt，表示main包下定义的NewInt类型。
2. b的类型是int。
3. MyInt类型只会在代码中存在，编译完成时并不会有MyInt类型。
:::
##  4. 结构体的定义
::: tip 提示
1. 语言内置的基础数据类型是用来描述`一个值`的，而<font color="color"><b>结构体是用来描述一组值</b></font>的。比如一个人有名字、年龄和居住城市等，本质上是一种聚合型的数据类型
2. 使用`type`和`struct`关键字来定义结构体，具体代码格式如下
:::
```shell
type 类型名 struct {
    字段名 字段类型
    字段名 字段类型
    …
}
```
1. 类型名：标识自定义结构体的名称，在同一个包内不能重复。
2. 字段名：表示结构体字段名。结构体中的字段名必须唯一。
3. 字段类型：表示结构体字段的具体类型。

<font color="blue"><b>举个例子，我们定义一个Person（人）结构体，代码如下：</b></font>
```go
package main

import (
	"fmt"
	"reflect"
)

// 1. 定义一个Person（人）结构体
type person struct {
	name string
	city string
	age  int8
}

func main() {
	// 2. 结构体实例化
	var p person
	// 3. `.`来访问结构体的字段
	p.name = "少年"
	p.city = "中国"
	p.age = 18
	fmt.Println(p)                 // {少年 中国 18}
	fmt.Println(reflect.TypeOf(p)) // main.person
}
```
<font color="blue"><b>同样类型的字段也可以写在一行：</b></font>
```go
type person1 struct {
	name, city string
	age        int8
}
```
::: warning 提示
1. 这样我们就拥有了一个`person`的自定义类型，它有`name`、`city`、`age`三个字段，分别表示姓名、城市和年龄。这样我们使用这个`person`结构体就能够很方便的在程序中表示和存储人信息了。
2. 语言内置的基础数据类型是用来描述`一个值`的，而<font color="color"><b>结构体是用来描述一组值</b></font>的。比如一个人有名字、年龄和居住城市等，本质上是一种聚合型的数据类型
:::
##  5. 结构体实例化
::: tip 提示
1. 只有当结构体实例化时，才会真正地分配内存。也就是必须实例化后才能使用结构体的字段。
2. 结构体本身也是一种类型，我们可以像声明内置类型一样使用`var关键字`声明结构体类型。
:::
```shell
var 结构体实例 结构体类型   
```
### 5.1 基本实例化
::: tip 提示
我们通过`.`来访问结构体的字段（成员变量）,例如`p.name`和`p.age`等。
:::
```go
package main

import (
	"fmt"
	"reflect"
)

// 1. 定义一个Person（人）结构体
type person struct {
	name string
	city string
	age  int8
}

func main() {
	// 2. 结构体实例化,p是实例,person是结构体类型
	var p person
	// 3. `.`来访问结构体的字段
	p.name = "少年"
	p.city = "中国"
	p.age = 18
	fmt.Println(p)                 // {少年 中国 18}
	fmt.Println(reflect.TypeOf(p)) // main.person
}
```
### 5.2 匿名结构体
在定义一些临时数据结构等场景下还可以使用匿名结构体。
```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	var user struct {
		Name string
		Age  int
	}
	user.Name = "小王子"
	user.Age = 18
	fmt.Println(user)                 // {小王子 18}
	fmt.Println(reflect.TypeOf(user)) // struct { Name string; Age int }
}
```
### 5.3 创建指针类型结构体
::: tip 提示
1. 使用`new`关键字对结构体进行实例化，得到的是结构体的地址
2. 支持对结构体指针直接使用`.`来访问结构体的成员
:::
```go
package main

import (
	"fmt"
	"reflect"
)

type person struct {
	name string
	city string
	age  int8
}

func main() {
	var p = new(person)
	fmt.Println(reflect.TypeOf(p)) // *main.person 是一个结构体指针
	p.name = "张三"
	p.city = "北京"
	p.age = 18
	fmt.Println(p)                 // &{张三 北京 18}
	fmt.Println(reflect.TypeOf(p)) // *main.person
}
```
### 5.4 取结构体的地址实例化
::: tip 提示
1. 使用`&`对结构体进行取地址操作相当于对该结构体类型进行了一次new实例化操作
2. 下面例子: `p3.name = "七米"`其实在底层是`(*p3).name = "七米"`，这是Go语言帮我们实现的语法糖。
:::
```go
package main

import (
	"fmt"
)

type person struct {
	name string
	city string
	age  int8
}

func main() {
	p3 := &person{}
	fmt.Printf("%T\n", p3)     //*main.person
	fmt.Printf("p3=%#v\n", p3) //p3=&main.person{name:"", city:"", age:0}
	p3.name = "七米"
	p3.age = 30
	p3.city = "成都"
	fmt.Printf("p3=%#v\n", p3) //p3=&main.person{name:"七米", city:"成都", age:30}
}
```
##  6. 结构体初始化
::: tip 提示
没有初始化的结构体，其成员变量都是对应其类型的零值。
:::
```go
package main

import (
	"fmt"
)

type person struct {
	name string
	city string
	age  int8
}

func main() {
	var p person              //实例化
	fmt.Println(p)            // {  0}
	fmt.Printf("p4=%#v\n", p) //p4=main.person{name:"", city:"", age:0}
}
```
### 6.1 使用键值对初始化
<font color="blue"><b>使用键值对对结构体进行初始化时，键对应结构体的字段，值对应该字段的初始值。</b></font>
```go
package main

import (
	"fmt"
)

type person struct {
	name string
	city string
	age  int8
}

func main() {
	p5 := person{
		name: "小王子",
		city: "北京",
		age:  18,
	}
	fmt.Printf("p5=%#v\n", p5) //p5=main.person{name:"小王子", city:"北京", age:18}
}
```
<font color="blue"><b>也可以对结构体指针进行键值对初始化，例如：</b></font>
```go
package main

import (
	"fmt"
)

type person struct {
	name string
	city string
	age  int8
}

func main() {
	p6 := &person{
		name: "小王子",
		city: "北京",
		age:  18,
	}
	fmt.Printf("p6=%#v\n", p6) //p6=&main.person{name:"小王子", city:"北京", age:18}
}
```
<font color="blue"><b>当某些字段没有初始值的时候，该字段可以不写。此时，没有指定初始值的字段的值就是该字段类型的零值。</b></font>
```go
package main

import (
	"fmt"
)

type person struct {
	name string
	city string
	age  int8
}

func main() {
	p7 := &person{
		city: "北京",
	}
	fmt.Printf("p7=%#v\n", p7) //p7=&main.person{name:"", city:"北京", age:0}
}
```
### 6.2 使用值的列表初始化
::: tip 提示
初始化结构体的时候可以简写，也就是初始化的时候不写键，直接写值：
:::
```go
package main

import (
	"fmt"
)

type person struct {
	name string
	city string
	age  int8
}

func main() {
	p8 := &person{
		"沙河娜扎",
		"北京",
		28,
	}
	fmt.Printf("p8=%#v\n", p8) //p8=&main.person{name:"沙河娜扎", city:"北京", age:28}
}
```
::: danger 使用这种格式初始化时，需要注意：
1. 必须初始化结构体的所有字段。
2. 初始值的填充顺序必须与字段在结构体中的声明顺序一致
3. 该方式不能和键值初始化方式混用
:::
##  7. 结构体内存布局
::: tip 注意
1. 结构体占用一块连续的内存。
2. 空结构体是不占用空间的。
:::
```go
package main

import (
	"fmt"
	"unsafe"
)

type test struct {
	a int8
	b int8
	c int8
	d int8
}

func main() {
	n := test{
		1, 2, 3, 4,
	}
	fmt.Printf("n.a %p\n", &n.a) // n.a 0xc0000aa058
	fmt.Printf("n.b %p\n", &n.b) // n.b 0xc0000aa059
	fmt.Printf("n.c %p\n", &n.c) // n.c 0xc0000aa05a
	fmt.Printf("n.d %p\n", &n.d) // n.d 0xc0000aa05b
	var v struct{}
	fmt.Println(unsafe.Sizeof(v)) // 0 空结构体是不占用空间的。
}
```
##  8. 构造函数
::: tip 提示
Go语言的结构体没有构造函数，我们可以自己实现。 例如，下方的代码就实现了一个person的构造函数。 因为struct是值类型，如果结构体比较复杂的话，值拷贝性能开销会比较大，所以该构造函数返回的是结构体指针类型。
:::
```go
package main

import (
	"fmt"
)

type person struct {
	name string
	city string
	age  int8
}

func newPerson(name, city string, age int8) *person {
	return &person{
		name: name,
		city: city,
		age:  age,
	}
}

func main() {
	// 调用构造函数
	p9 := newPerson("张三", "沙河", 90)
	fmt.Printf("%#v\n", p9) //&main.person{name:"张三", city:"沙河", age:90}
}
```
##  9. 方法和接收者
::: tip 提示
1. 方法与函数的区别是，函数不属于任何类型，方法属于特定的类型。
2. Go语言中的方法（Method）是一种作用于特定类型变量的函数。这种特定类型变量叫做接收者（Receiver）。接收者的概念就类似于其他语言中的this或者 self。
:::
<font color="blue"><b>方法的定义格式如下：</b></font>
```shell
func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
    函数体
}
```
1. `接收者变量`：接收者中的参数变量名在命名时，官方建议使用接收者类型名称首字母的小写，而不是self、this之类的命名。例如，Person类型的接收者变量应该命名为 p，Connector类型的接收者变量应该命名为c等。
2. `接收者类型`：接收者类型和参数类似，可以是指针类型和非指针类
3. 方法名、参数列表、返回参数：具体格式与函数定义相同。
```go
package main

import (
	"fmt"
)

//Person 结构体
type Person struct {
	name string
	age  int8
}

//NewPerson 构造函数
func NewPerson(name string, age int8) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}

//Dream Person做梦的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Go语言！\n", p.name)
}

func main() {
	p1 := NewPerson("小王子", 25)
	p1.Dream()
}
```
### 9.1 指针类型的接收者
::: tip 提示
1. 指针类型的接收者由一个结构体的指针组成，由于指针的特性，调用方法时修改接收者指针的任意成员变量，在方法结束后，修改都是有效的。
2. 这种方式就十分接近于其他语言中面向对象中的this或者self。 
3. 例如我们为Person添加一个SetAge方法，来修改实例变量的年龄。
:::
```go
package main

import "fmt"

//Person 结构体
type Person struct {
	name string
	age  int8
}

//NewPerson 构造函数
func NewPerson(name string, age int8) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}

// SetAge 设置p的年龄
// 使用指针接收者
func (p *Person) SetAge(newAge int8) {
	p.age = newAge
}

func main() {
	p1 := NewPerson("小王子", 25)
	fmt.Println(p1.age) // 25
	p1.SetAge(30)       // 调用方法设置年龄
	fmt.Println(p1.age) // 30
}
```
### 9.2 值类型的接收者
::: tip 提示
1. 当方法作用于值类型接收者时，Go语言会在代码运行时将接收者的值复制一份。
2. 在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是针对副本，无法修改接收者变量本身。
:::
```go
package main

import "fmt"

//Person 结构体
type Person struct {
	name string
	age  int8
}

//Dream Person做梦的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Go语言！\n", p.name)
}

//NewPerson 构造函数
func NewPerson(name string, age int8) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}

// SetAge2 设置p的年龄
// 使用值接收者
func (p Person) SetAge2(newAge int8) {
	p.age = newAge
}
func main() {
	p1 := NewPerson("小王子", 25)
	p1.Dream()
	fmt.Println(p1.age) // 25
	p1.SetAge2(30)      // (*p1).SetAge2(30)
	fmt.Println(p1.age) // 25
}
```
### 9.3 什么时候应该使用指针类型接收者
::: danger 重点来了
1. 需要修改接收者中的值
2. 接收者是拷贝代价比较大的大对象
3. 保证一致性，如果有某个方法使用了指针接收者，那么其他的方法也应该使用指针接收者。
:::
##  10. 任意类型添加方法
::: danger 注意
1. 注意事项： 非本地类型不能定义方法，也就是说我们不能给别的包的类型定义方法。
2. 在Go语言中，接收者的类型可以是任何类型，不仅仅是`结构体`，`任何类型都可以拥有方法`。
3. 举个例子，我们基于内置的int类型使用type关键字可以定义新的自定义类型，然后为我们的自定义类型添加方法。
:::
```go
package main

import "fmt"

//MyInt 将int定义为自定义MyInt类型
type MyInt int

//SayHello 为MyInt添加一个SayHello的方法
func (m MyInt) SayHello() {
	fmt.Println("Hello, 我是一个int。")
}
func main() {
	var m1 MyInt
	m1.SayHello() //Hello, 我是一个int。
	m1 = 100
	fmt.Printf("%#v  %T\n", m1, m1) //100  main.MyInt
}
```
##  11. 结构体的匿名字段
::: tip 提示
1. 结构体允许其成员字段在声明时没有字段名而只有类型，这种没有名字的字段就称为匿名字段。
2. 注意：这里匿名字段的说法并不代表没有字段名，而是默认会采用类型名作为字段名，结构体要求字段名称必须唯一，因此一个结构体中同种类型的匿名字段只能有一个。
:::
```shell
package main

import "fmt"

//Person 结构体Person类型
type Person struct {
	string
	int
}

func main() {
	p1 := Person{
		"小王子",
		18,
	}
	fmt.Printf("%#v\n", p1)        //main.Person{string:"北京", int:18}
	fmt.Println(p1.string, p1.int) //北京 18
}
```
##  12. 嵌套结构体
一个结构体中可以嵌套包含另一个结构体或结构体指针，就像下面的示例代码那样。
```go
package main

import "fmt"

//Address 地址结构体
type Address struct {
	Province string
	City     string
}

//User 用户结构体
type User struct {
	Name    string
	Gender  string
	Address Address
}

func main() {
	user1 := User{
		Name:   "小王子",
		Gender: "男",
		Address: Address{
			Province: "山东",
			City:     "威海",
		},
	}
	/*user1=main.User{
	    Name:"小王子", 
	    Gender:"男", 
	    Address:main.Address{Province:"山东", City:"威海"}
	 }
	*/
	fmt.Printf("user1=%#v\n", user1)
}
```
###  12.1 嵌套匿名字段
::: tip 提示
1. 上面user结构体中嵌套的Address结构体也可以采用匿名字段的方式
2. 当访问结构体成员时会先在结构体中查找该字段，找不到再去嵌套的匿名字段中查找。
:::
```go
package main

import "fmt"

//Address 地址结构体
type Address struct {
	Province string
	City     string
}

//User 用户结构体
type User struct {
	Name    string
	Gender  string
	Address //匿名字段
}

func main() {
	var user2 User
	user2.Name = "小王子"
	user2.Gender = "男"
	user2.Address.Province = "山东" // 匿名字段默认使用类型名作为字段名
	user2.City = "威海"             // 匿名字段可以省略
	/* user2=main.User{
		Name:"小王子",
		Gender:"男",
		Address:main.Address{Province:"山东", City:"威海"}
	}
	*/
	fmt.Printf("user2=%#v\n", user2)
}
```
### 12.2 嵌套结构体的字段名冲突
::: tip 提示
嵌套结构体内部可能存在相同的字段名。在这种情况下为了避免歧义需要通过指定具体的内嵌结构体字段名。
:::
```go
package main

//Address 地址结构体
type Address struct {
	Province   string
	City       string
	CreateTime string
}

//Email 邮箱结构体
type Email struct {
	Account    string
	CreateTime string
}

//User 用户结构体
type User struct {
	Name   string
	Gender string
	Address
	Email
}

func main() {
	var user3 User
	user3.Name = "沙河娜扎"
	user3.Gender = "男"
	// user3.CreateTime = "2019" //ambiguous selector user3.CreateTime
	user3.Address.CreateTime = "2000" //指定Address结构体中的CreateTime
	user3.Email.CreateTime = "2000"   //指定Email结构体中的CreateTime
}
```
##  13. 结构体的“继承”
::: tip 提示
Go语言中使用结构体也可以实现其他编程语言中面向对象的继承。
:::
```go
package main

import "fmt"

//Animal 动物
type Animal struct {
	name string
}

func (a *Animal) move() {
	fmt.Printf("%s会动！\n", a.name)
}

//Dog 狗
type Dog struct {
	Feet    int8
	*Animal //通过嵌套匿名结构体实现继承
}

func (d *Dog) wang() {
	fmt.Printf("%s会汪汪汪~\n", d.name)
}

func main() {
	d1 := &Dog{
		Feet: 4,
		Animal: &Animal{ //注意嵌套的是结构体指针
			name: "乐乐",
		},
	}
	d1.wang() //乐乐会汪汪汪~
	d1.move() //乐乐会动！
}
```
##  14. 结构体字段的可见性
::: danger 注意
结构体中字段大写开头表示可公开访问，小写表示私有（仅在定义当前结构体的包中可访问）。
:::
##  15. 结构体与JSON序列化
::: danger 注意
1. JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。易于人阅读和编写。同时也易于机器解析和生成。
2. JSON键值对是用来保存JS对象的一种方式，键/值对组合中的键名写在前面并用双引号`""`包裹，使用冒号`:`分隔，然后紧接着值；多个键值之间使用英文`,`分隔。
:::
```go
package main

import (
	"encoding/json"
	"fmt"
	"reflect"
)

//Student 学生
type Student struct {
	ID     int
	Gender string
	Name   string
}

//Class 班级
type Class struct {
	Title    string
	Students []*Student
}

func main() {
	c := &Class{
		Title:    "101",
		Students: make([]*Student, 0, 200),
	}
	for i := 0; i < 2; i++ {
		stu := &Student{
			Name:   fmt.Sprintf("stu%02d", i),
			Gender: "男",
			ID:     i,
		}
		c.Students = append(c.Students, stu)
	}
	//JSON序列化：结构体-->JSON格式的字符串
	data, err := json.Marshal(c)
	fmt.Println("JSON序列化", reflect.TypeOf(data))
	if err != nil {
		fmt.Println("json marshal failed")
		return
	}
	fmt.Printf("json:%s\n", data)
	//JSON反序列化：JSON格式的字符串-->结构体
	str := `{"Title":"101","Students":[
		{"ID":0,"Gender":"男","Name":"stu00"},
		{"ID":1,"Gender":"男","Name":"stu01"}
		]}`
	c1 := &Class{}
	err = json.Unmarshal([]byte(str), c1)
	if err != nil {
		fmt.Println("json unmarshal failed!")
		return
	}
	fmt.Println("JSON反序列化", reflect.TypeOf(c1))
	fmt.Printf("%#v\n", c1)
}
```
##  16. 结构体标签（Tag）
::: tip 提示
Tag是结构体的元信息，可以在运行的时候通过反射的机制读取出来。 Tag在结构体字段的后方定义，由一对反引号包裹起来，具体的格式如下：
:::
```shell
`key1:"value1" key2:"value2"`
```
1. 结构体`tag`由一个或多个键值对组成。`键与值`使用冒号分隔，`值`用双引号括起来。
2. 同一个结构体字段可以设置多个键值对tag，不同的键值对之间使用空格分隔。
::: danger 注意事项
1. 为结构体编写Tag时，必须严格遵守键值对的规则。
2. 结构体标签的解析代码的容错能力很差，一旦格式写错，编译和运行时都不会提示任何错误，通过反射也无法正确取值。例如不要在key和value之间添加空格。
:::
例如我们为Student结构体的每个字段定义json序列化时使用的Tag：
```go
package main

import (
	"encoding/json"
	"fmt"
)

//Student 学生
type Student struct {
	ID     int    `json:"id"` //通过指定tag实现json序列化该字段时的key
	Gender string //json序列化是默认使用字段名作为key
	name   string //私有不能被json包访问
}

func main() {
	s1 := Student{
		ID:     1,
		Gender: "男",
		name:   "沙河娜扎",
	}
	data, err := json.Marshal(s1)
	if err != nil {
		fmt.Println("json marshal failed!")
		return
	}
	fmt.Printf("json str:%s\n", data) //json str:{"id":1,"Gender":"男"}
}
```
## 17. 结构体和方法补充知识点
::: tip 提示
因为slice和map这两种数据类型都包含了指向底层数据的指针，因此我们在需要复制它们时要特别注意。我们来看下面的例子：
:::
```go
package main

import "fmt"

type Person struct {
	name   string
	age    int8
	dreams []string
}

func (p *Person) SetDreams(dreams []string) {
	p.dreams = dreams
}

func main() {
	p1 := Person{name: "小王子", age: 18}
	data := []string{"吃饭", "睡觉", "打豆豆"}
	p1.SetDreams(data)

	// 你真的想要修改 p1.dreams 吗？
	data[1] = "不睡觉"
	fmt.Println(p1.dreams) // ? [吃饭 不睡觉 打豆豆]
}
```

正确的做法是在方法中使用传入的slice的拷贝进行结构体赋值。
```go
package main

import "fmt"

type Person struct {
	name   string
	age    int8
	dreams []string
}

func (p *Person) SetDreams(dreams []string) {
	p.dreams = make([]string, len(dreams))
	copy(p.dreams, dreams)
}

func main() {
	p1 := Person{name: "小王子", age: 18}
	data := []string{"吃饭", "睡觉", "打豆豆"}
	p1.SetDreams(data)

	// 你真的想要修改 p1.dreams 吗？
	data[1] = "不睡觉"
	fmt.Println(p1.dreams) // [吃饭 睡觉 打豆豆]
}
```
同样的问题也存在于返回值slice和map的情况，在实际编码过程中一定要注意这个问题。