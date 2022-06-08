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

