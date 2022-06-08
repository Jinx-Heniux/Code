---
description: 嵌入字段
---

# 匿名字段

## [匿名字段 · Go语言中文文档](https://www.topgoer.com/%E6%96%B9%E6%B3%95/%E5%8C%BF%E5%90%8D%E5%AD%97%E6%AE%B5.html)

### 像字段成员那样访问匿名字段方法

```go
package main

import "fmt"

type User struct {
	id   int
	name string
}

type Manager struct {
	User
}

func (self *User) ToString() string { // receiver = &(Manager.User)
	return fmt.Sprintf("User: %p, %v", self, self)
}

func main() {
	m := Manager{User{1, "Tom"}}
	fmt.Printf("Manager: %p\n", &m)
	fmt.Println(m.ToString())
}

/*
Manager: 0xc00000c030
User: 0xc00000c030, &{1 Tom}
*/

```



### 继承类似 override

```go
package main

import "fmt"

type User struct {
	id   int
	name string
}

type Manager struct {
	User
	title string
}

func (self *User) ToString() string {
	return fmt.Sprintf("User: %p, %v", self, self)
}

func (self *Manager) ToString() string {
	return fmt.Sprintf("Manager: %p, %v", self, self)
}

func main() {
	m := Manager{User{1, "Tom"}, "Administrator"}

	fmt.Println(m.ToString())

	fmt.Println(m.User.ToString())
}

/*
Manager: 0xc0000a0150, &{{1 Tom} Administrator}
User: 0xc0000a0150, &{1 Tom}
*/

```



## [匿名字段 · Go语言中文文档](https://www.topgoer.com/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1/%E5%8C%BF%E5%90%8D%E5%AD%97%E6%AE%B5.html)



```go
package main

import "fmt"

//    go支持只提供类型而不写字段名的方式，也就是匿名字段，也称为嵌入字段

//人
type Person struct {
	name string
	sex  string
	age  int
}

type Student struct {
	Person
	id   int
	addr string
}

func main() {
	// 初始化
	s1 := Student{Person{"5lmh", "man", 20}, 1, "bj"}
	fmt.Println(s1)

	s2 := Student{Person: Person{"5lmh", "man", 20}}
	fmt.Println(s2)

	s3 := Student{Person: Person{name: "5lmh"}}
	fmt.Println(s3)
}

/*
{{5lmh man 20} 1 bj}
{{5lmh man 20} 0 }
{{5lmh  0} 0 }
*/

```

