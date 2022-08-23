---
description: 方法
---

# Method

## [方法定义 · Go语言中文文档](https://www.topgoer.com/%E6%96%B9%E6%B3%95/%E6%96%B9%E6%B3%95%E5%AE%9A%E4%B9%89.html)

### 定义一个结构体类型和该类型的一个方法

```go
package main

import (
	"fmt"
)

//结构体
type User struct {
	Name  string
	Email string
}

//方法
func (u User) Notify() {
	fmt.Printf("in Notify: %v, %v, %T, %p \n", u.Name, u.Email, u, &u)
}

func main() {
	// 值类型调用方法
	u1 := User{"golang", "golang@golang.com"}
	fmt.Printf("u1 in main: %v, %v, %T, %p \n", u1.Name, u1.Email, u1, &u1)
	u1.Notify()
	// 指针类型调用方法
	fmt.Println()
	u2 := User{"go", "go@go.com"}
	u3 := &u2
	fmt.Printf("u2 in main: %v, %v, %T, %p \n", u2.Name, u2.Email, u2, &u2)
	fmt.Printf("u3 in main: %v, %v, %T, %p,%p\n", u3.Name, u3.Email, u3, u3, &u3)
	u3.Notify()
}

/*
u1 in main: golang, golang@golang.com, main.User, 0xc0000ba000
in Notify: golang, golang@golang.com, main.User, 0xc0000ba040

u2 in main: go, go@go.com, main.User, 0xc0000ba080
u3 in main: go, go@go.com, *main.User, 0xc0000ba080,0xc0000b4020
in Notify: go, go@go.com, main.User, 0xc0000ba0c0
*/


```



### 接受者使用指针类型

```go
package main

import (
    "fmt"
)

//结构体
type User struct {
    Name  string
    Email string
}

//方法
func (u *User) Notify() {
    fmt.Printf("%v : %v \n", u.Name, u.Email)
}
func main() {
    // 值类型调用方法
    u1 := User{"golang", "golang@golang.com"}
    u1.Notify()
    // 指针类型调用方法
    u2 := User{"go", "go@go.com"}
    u3 := &u2
    u3.Notify()
}
```



### receiver T 和 \*T 的差别

```go
package main

import "fmt"

type Data struct {
	x int
}

func (self Data) ValueTest() { // func ValueTest(self Data);
	fmt.Printf("Value: %p\n", &self)
}

func (self *Data) PointerTest() { // func PointerTest(self *Data);
	fmt.Printf("Pointer: %p\n", self)
}

func main() {
	d := Data{}
	p := &d
	fmt.Printf("Data: %v | %p\n", d, p)

	d.ValueTest()   // ValueTest(d)
	d.PointerTest() // PointerTest(&d)

	p.ValueTest()   // ValueTest(*p)
	p.PointerTest() // PointerTest(p)
}

/*
Data: {0} | 0xc0000ba000
Value: 0xc0000ba020
Pointer: 0xc0000ba000
Value: 0xc0000ba028
Pointer: 0xc0000ba000
*/
```



### 普通函数与方法的区别

```go
package main

//普通函数与方法的区别（在接收者分别为值类型和指针类型的时候）

import (
	"fmt"
)

//1.普通函数
//接收值类型参数的函数
func valueIntTest(a int) int {
	return a + 10
}

//接收指针类型参数的函数
func pointerIntTest(a *int) int {
	return *a + 10
}

func structTestValue() {
	a := 2
	fmt.Println("valueIntTest:", valueIntTest(a))
	//函数的参数为值类型，则不能直接将指针作为参数传递
	//fmt.Println("valueIntTest:", valueIntTest(&a))
	//compile error: cannot use &a (type *int) as type int in function argument

	b := 5
	fmt.Println("pointerIntTest:", pointerIntTest(&b))
	//同样，当函数的参数为指针类型时，也不能直接将值类型作为参数传递
	//fmt.Println("pointerIntTest:", pointerIntTest(b))
	//compile error:cannot use b (type int) as type *int in function argument
}

//2.方法
type PersonD struct {
	id   int
	name string
}

//接收者为值类型
func (p PersonD) valueShowName() {
	fmt.Println(p.name)
}

//接收者为指针类型
func (p *PersonD) pointShowName() {
	fmt.Println(p.name)
}

func structTestFunc() {
	//值类型调用方法
	personValue := PersonD{101, "hello world"}
	personValue.valueShowName()
	personValue.pointShowName()

	//指针类型调用方法
	personPointer := &PersonD{102, "hello golang"}
	personPointer.valueShowName()
	personPointer.pointShowName()

	//与普通函数不同，接收者为指针类型和值类型的方法，指针类型和值类型的变量均可相互调用
}

func main() {
	structTestValue()
	structTestFunc()
}

/*
valueIntTest: 12
pointerIntTest: 15
hello world
hello world
hello golang
hello golang
*/

```

