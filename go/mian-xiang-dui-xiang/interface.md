---
description: 接口
---

# Interface

## [接口 · Go语言中文文档](https://www.topgoer.com/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1/%E6%8E%A5%E5%8F%A3.html)

### 面试题 请问下面的代码是否能通过编译？

```go
package main

import "fmt"

type People interface {
	Speak(string) string
}

type Student struct{}

func (stu *Student) Speak(think string) (talk string) {
	if think == "sb" {
		talk = "你是个大帅比"
	} else {
		talk = "您好"
	}
	return
}

func main() {
	var peo People = Student{}
	think := "bitch"
	fmt.Println(peo.Speak(think))
}

/*
main.Student
cannot use (Student literal) (value of type Student) as People value in variable declaration:
Student does not implement People (method Speak has pointer receiver)

修改1
func (stu Student) Speak(think string) (talk string) {

修改2
var peo People = &Student{}
*/

```



### 一个类型实现多个接口

```go
package main

import "fmt"

// Sayer 接口
type Sayer interface {
	say()
}

// Mover 接口
type Mover interface {
	move()
}

type dog struct {
	name string
}

// 实现Sayer接口
func (d dog) say() {
	fmt.Printf("%s会叫汪汪汪\n", d.name)
}

// 实现Mover接口
func (d dog) move() {
	fmt.Printf("%s会动\n", d.name)
}

func main() {
	var x Sayer
	var y Mover

	var a = dog{name: "旺财"}
	x = a
	y = a
	x.say()
	y.move()
}

/*
旺财会叫汪汪汪
旺财会动
*/

```



### 多个类型实现同一接口

```go
package main

import "fmt"

// Mover 接口
type Mover interface {
	move()
}

type dog struct {
	name string
}

type car struct {
	brand string
}

// dog类型实现Mover接口
func (d dog) move() {
	fmt.Printf("%s会跑\n", d.name)
}

// car类型实现Mover接口
func (c car) move() {
	fmt.Printf("%s速度70迈\n", c.brand)
}

func main() {
	var x Mover
	var a = dog{name: "旺财"}
	var b = car{brand: "保时捷"}
	x = a
	x.move()
	x = b
	x.move()
}

/*
旺财会跑
保时捷速度70迈
*/

```



### 接口嵌套

```go
package main

import "fmt"

// Sayer 接口
type Sayer interface {
	say()
}

// Mover 接口
type Mover interface {
	move()
}

// 接口嵌套
type animal interface {
	Sayer
	Mover
}

type cat struct {
	name string
}

func (c cat) say() {
	fmt.Println("喵喵喵")
}

func (c cat) move() {
	fmt.Println("猫会动")
}

func main() {
	var x animal
	x = cat{name: "花花"}
	x.move()
	x.say()
}

/*
猫会动
喵喵喵
*/

```



### 空接口

```go
package main

import "fmt"

func main() {
	// 定义一个空接口x
	var x interface{}
	s := "pprof.cn"
	x = s
	fmt.Printf("type:%T value:%v\n", x, x)
	i := 100
	x = i
	fmt.Printf("type:%T value:%v\n", x, x)
	b := true
	x = b
	fmt.Printf("type:%T value:%v\n", x, x)
}

/*
type:string value:pprof.cn
type:int value:100
type:bool value:true
*/

```



### 空接口作为函数的参数

```go
package main

import "fmt"

// 空接口作为函数参数
func show(a interface{}) {
	fmt.Printf("type:%T value:%v\n", a, a)
}

func main() {

	show("pprof.cn")
	show(100)
	show(true)
}

/*
type:string value:pprof.cn
type:int value:100
type:bool value:true
*/

```



### 空接口作为map的值

```go
package main

import "fmt"

func main() {

	// 空接口作为map值
	var studentInfo = make(map[string]interface{})
	studentInfo["name"] = "李白"
	studentInfo["age"] = 18
	studentInfo["married"] = false
	fmt.Println(studentInfo)
}

/*
map[age:18 married:false name:李白]
*/

```



### 类型断言 接口值

```go
package main

import "fmt"

func main() {
	var x interface{}
	x = "pprof.cn"
	v, ok := x.(string)
	if ok {
		fmt.Println(v)
	} else {
		fmt.Println("类型断言失败")
	}
}

// pprof.cn

```

