# 方法

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

