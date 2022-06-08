---
description: 结构体
---

# Struct

## [结构体 · Go语言中文文档](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E7%BB%93%E6%9E%84%E4%BD%93.html)

### 类型定义和类型别名的区别

```go
package main

import (
	"fmt"
)

//类型定义
type NewInt int

//类型别名
type MyInt = int

func main() {
	var a NewInt
	var b MyInt

	fmt.Printf("type of a: %T\n", a) //type of a:main.NewInt
	fmt.Printf("type of b: %T\n", b) //type of b:int
}

// 结果显示a的类型是main.NewInt，表示main包下定义的NewInt类型。
// b的类型是int。MyInt类型只会在代码中存在，编译完成时并不会有MyInt类型。

```



### 结构体实例化

```go
package main

import (
	"fmt"
)

type person struct {
	name string // 结构体的字段（成员变量）
	city string
	age  int8
}

func main() {
	// 结构体初始化
	var p1 person
	fmt.Printf("p1=%#v\n", p1)
	//p1=main.person{name:"", city:"", age:0}

	p1.name = "pprof.cn"
	p1.city = "北京"
	p1.age = 18
	fmt.Printf("p1=%v\n", p1) //p1={pprof.cn 北京 18}
	fmt.Printf("p1=%#v\n", p1)
	//p1=main.person{name:"pprof.cn", city:"北京", age:18}
}

```

### 匿名结构体

```go
package main

import (
	"fmt"
)

func main() {
	var user struct {
		Name string
		Age  int
	}
	user.Name = "pprof.cn"
	user.Age = 18
	fmt.Printf("%#v\n", user) // struct { Name string; Age int }{Name:"pprof.cn", Age:18}
}

```

### 创建指针类型结构体

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
	// 可以通过使用new关键字对结构体进行实例化，得到的是结构体的地址。
	var p2 = new(person)
	fmt.Printf("%T\n", p2)     //*main.person // p2是一个结构体指针
	fmt.Printf("p2=%#v\n", p2) //p2=&main.person{name:"", city:"", age:0}

	p2.name = "测试"
	p2.age = 18
	p2.city = "北京"
	fmt.Printf("p2=%#v\n", p2) //p2=&main.person{name:"测试", city:"北京", age:18}
}

```



### 取结构体的地址实例化

### 使用键值对初始化

### 使用值的列表初始化

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

	/* 取结构体的地址实例化 */
	p3 := &person{}
	// 使用&对结构体进行取地址操作相当于对该结构体类型进行了一次new实例化操作。
	fmt.Printf("%T\n", p3)     //*main.person
	fmt.Printf("p3=%#v\n", p3) //p3=&main.person{name:"", city:"", age:0}

	p3.name = "博客"
	// p3.name = "博客"其实在底层是(*p3).name = "博客"，这是Go语言帮我们实现的语法糖。
	p3.age = 30
	p3.city = "成都"
	fmt.Printf("p3=%#v\n", p3)
	//p3=&main.person{name:"博客", city:"成都", age:30}

	/* 使用键值对初始化 */
	// 使用键值对对结构体进行初始化时，键对应结构体的字段，值对应该字段的初始值。
	p5 := person{
		name: "pprof.cn",
		city: "北京",
		age:  18,
	}
	fmt.Printf("p5=%#v\n", p5) //p5=main.person{name:"pprof.cn", city:"北京", age:18}

	// 也可以对结构体指针进行键值对初始化
	p6 := &person{
		name: "pprof.cn",
		city: "北京",
		age:  18,
	}
	fmt.Printf("p6=%#v\n", p6) //p6=&main.person{name:"pprof.cn", city:"北京", age:18}

	p7 := &person{
		city: "北京",
	}
	fmt.Printf("p7=%#v\n", p7) //p7=&main.person{name:"", city:"北京", age:0}

	/* 使用值的列表初始化 */
	p8 := &person{
		"pprof.cn",
		"北京",
		18,
	}
	fmt.Printf("p8=%#v\n", p8) //p8=&main.person{name:"pprof.cn", city:"北京", age:18}
}
```



### 结构体内存布局

```go
package main

import (
	"fmt"
)

func main() {

	// 注意：结构体可以被定义在函数中
	type test struct {
		a int8
		b int8
		c int8
		d int8
	}
	n := test{
		1, 2, 3, 4,
	}
	fmt.Printf("n.a %p\n", &n.a)
	fmt.Printf("n.b %p\n", &n.b)
	fmt.Printf("n.c %p\n", &n.c)
	fmt.Printf("n.d %p\n", &n.d)

	// n.a 0xc0000c0000
	// n.b 0xc0000c0001
	// n.c 0xc0000c0002
	// n.d 0xc0000c0003

}

```

### 面试题

```go
package main

import (
	"fmt"
)

type student struct {
	name string
	age  int
}

func main() {
	m := make(map[string]*student)
	stus := []student{
		{name: "pprof.cn", age: 18},
		{name: "测试", age: 23},
		{name: "博客", age: 28},
	}

	for i, stu := range stus {
		// fmt.Printf("stu.name: %v | stu: %v | stus[%d]: %v\n", stu.name, stu, i, stus[i])
		// fmt.Printf("addr stu: %p | addr of stus[%d]: %p\n", &stu, i, &stus[i])
		fmt.Printf("stus: %v | addr: %p\n", stus, &stus)
		fmt.Printf("\tstu: %v | addr: %p\n", stu, &stu)
		fmt.Printf("\tstus[%d]: %v | addr: %p\n", i, stus[i], &stus[i])
		fmt.Println()
		m[stu.name] = &stu
	}
	fmt.Println()
	for k, v := range m {
		fmt.Printf("%#v => %#v (%T) (%p) \n", k, v, v, v)
	}
}

/*
stus: [{pprof.cn 18} {测试 23} {博客 28}] | addr: 0xc00000c030
        stu: {pprof.cn 18} | addr: 0xc00000c048
        stus[0]: {pprof.cn 18} | addr: 0xc000064050

stus: [{pprof.cn 18} {测试 23} {博客 28}] | addr: 0xc00000c030
        stu: {测试 23} | addr: 0xc00000c048
        stus[1]: {测试 23} | addr: 0xc000064068

stus: [{pprof.cn 18} {测试 23} {博客 28}] | addr: 0xc00000c030
        stu: {博客 28} | addr: 0xc00000c048
        stus[2]: {博客 28} | addr: 0xc000064080


"测试" => &main.student{name:"博客", age:28} (*main.student) (0xc00000c048)
"博客" => &main.student{name:"博客", age:28} (*main.student) (0xc00000c048)
"pprof.cn" => &main.student{name:"博客", age:28} (*main.student) (0xc00000c048)
*/


```



### 构造函数

```go
package main

import "fmt"

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
	p9 := newPerson("pprof.cn", "测试", 90)
	fmt.Printf("%#v\n", p9)
}

// &main.person{name:"pprof.cn", city:"测试", age:90}

```



### 方法和接收者

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

//Dream Person做梦的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Go语言！\n", p.name)
}

func main() {
	p1 := NewPerson("测试", 25)
	p1.Dream() // 测试的梦想是学好Go语言！
}

// 方法与函数的区别是，函数不属于任何类型，方法属于特定的类型。

```



### 指针类型的接收者

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

//Dream Person做梦的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Go语言！\n", p.name)
}

// SetAge 设置p的年龄
// 使用指针接收者
func (p *Person) SetAge(newAge int8) {
	p.age = newAge
}

func main() {
	p1 := NewPerson("测试", 25)
	fmt.Println(p1.age) // 25
	p1.SetAge(30)
	fmt.Println(p1.age) // 30
}

```



### 值类型的接收者

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

//Dream Person做梦的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Go语言！\n", p.name)
}

// SetAge2 设置p的年龄
// 使用值接收者
func (p Person) SetAge2(newAge int8) {
	p.age = newAge
}

func main() {
	p1 := NewPerson("测试", 25)
	p1.Dream()
	fmt.Println(p1.age) // 25
	p1.SetAge2(30)      // (*p1).SetAge2(30)
	fmt.Println(p1.age) // 25
}

/*
当方法作用于值类型接收者时，Go语言会在代码运行时将接收者的值复制一份。
在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是针对副本，无法修改接收者变量本身。
*/

```



### 任意类型添加方法

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



### 结构体的匿名字段

```go
package main

import "fmt"

//Person 结构体Person类型
type Person struct {
	string
	int
}

func main() {
	p1 := Person{
		"pprof.cn",
		18,
	}
	fmt.Printf("%#v\n", p1)        //main.Person{string:"pprof.cn", int:18}
	fmt.Println(p1.string, p1.int) //pprof.cn 18
}

// 匿名字段默认采用类型名作为字段名，结构体要求字段名称必须唯一，因此一个结构体中同种类型的匿名字段只能有一个。

```



### 嵌套结构体

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
		Name:   "pprof",
		Gender: "女",
		Address: Address{
			Province: "黑龙江",
			City:     "哈尔滨",
		},
	}
	fmt.Printf("user1=%#v\n", user1) //user1=main.User{Name:"pprof", Gender:"女", Address:main.Address{Province:"黑龙江", City:"哈尔滨"}}
}

```



### 嵌套匿名结构体

```go
package main

import "fmt"

//Address 地址结构体
type Address struct {
	Province string
	City     string
}

/*
type Address2 struct {
	Province string
	City     string
}
*/

//User 用户结构体
type User struct {
	Name    string
	Gender  string
	Address //匿名结构体
	// Address2
}

func main() {
	var user2 User
	user2.Name = "pprof"
	user2.Gender = "女"
	user2.Address.Province = "黑龙江"   //通过匿名结构体.字段名访问
	user2.City = "哈尔滨"               //直接访问匿名结构体的字段名
	fmt.Printf("user2=%#v\n", user2) //user2=main.User{Name:"pprof", Gender:"女", Address:main.Address{Province:"黑龙江", City:"哈尔滨"}}
}

/*
当访问结构体成员时会先在结构体中查找该字段，找不到再去匿名结构体中查找。
*/

```

