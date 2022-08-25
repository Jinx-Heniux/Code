# Expression

## [表达式 · Go语言中文文档](https://www.topgoer.com/%E6%96%B9%E6%B3%95/%E8%A1%A8%E8%BE%BE%E5%BC%8F.html)



```go
package main

import "fmt"

type User struct {
	id   int
	name string
}

func (self *User) Test() {
	fmt.Printf("%p, %v\n", self, self)
}

func main() {
	u := User{1, "Tom"}
	u.Test()

	mValue := u.Test
	fmt.Printf("mValue: %p,%T\n", mValue, mValue)
	mValue() // 隐式传递 receiver

	mExpression := (*User).Test
	fmt.Printf("mExpression: %p,%T\n", mExpression, mExpression)
	mExpression(&u) // 显式传递 receiver
}

/*
0xc0000ac018, &{1 Tom}
mValue: 0x47f760,func()
0xc0000ac018, &{1 Tom}
mExpression: 0x47f460,func(*main.User)
0xc0000ac018, &{1 Tom}
*/


```

