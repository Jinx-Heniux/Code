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



### method value 会复制 receiver

```go
package main

import "fmt"

type User struct {
	id   int
	name string
}

func (self User) Test() {
	fmt.Println(self)
}

func main() {
	u := User{1, "Tom"}
	mValue := u.Test // 立即复制 receiver，因为不是指针类型，不受后续修改影响。
	fmt.Printf("mValue: %p,%T\n", mValue, mValue)

	u.id, u.name = 2, "Jack"
	u.Test()

	mValue()
}

/*
mValue: 0x47f920,func()
{2 Jack}
{1 Tom}
*/


```



### method expression，注意 receiver 类型的差异

```go
package main

import "fmt"

type User struct {
	id   int
	name string
}

func (self *User) TestPointer() {
	fmt.Printf("TestPointer: %p, %v\n", self, self)
}

func (self User) TestValue() {
	fmt.Printf("TestValue: %p, %v\n", &self, self)
}

func main() {
	u := User{1, "Tom"}
	fmt.Printf("User u: %p, %v\n", &u, u)
	// u2 := User{2, "Tom2"}
	// fmt.Printf("User u2: %p, %v\n", &u2, u2)

	mv := User.TestValue
	fmt.Printf("mv: %p, %T\n", &mv, mv)
	mv(u)

	mp := (*User).TestPointer
	fmt.Printf("mp: %p, %T\n", &mp, mp)
	mp(&u)

	mp2 := (*User).TestValue // *User 方法集包含 TestValue。签名变为 func TestValue(self *User)。实际依然是 receiver value copy。
	fmt.Printf("mp2: %p, %T\n", &mp2, mp2)
	mp2(&u)
}

/*
User u: 0xc00000c030, {1 Tom}
mv: 0xc00000e030, func(main.User)
TestValue: 0xc00000c060, {1 Tom}
mp: 0xc00000e038, func(*main.User)
TestPointer: 0xc00000c030, &{1 Tom}
mp2: 0xc00000e040, func(*main.User)
TestValue: 0xc00000c0a8, {1 Tom}
*/


```

