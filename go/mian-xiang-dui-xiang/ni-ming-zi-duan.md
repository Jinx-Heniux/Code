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

