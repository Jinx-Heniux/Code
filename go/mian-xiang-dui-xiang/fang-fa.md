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

