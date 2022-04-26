# Struct

## 结构体 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E7%BB%93%E6%9E%84%E4%BD%93.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E7%BB%93%E6%9E%84%E4%BD%93.html)

### 结构体实例化

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
	var p1 person
	p1.name = "pprof.cn"
	p1.city = "北京"
	p1.age = 18
	fmt.Printf("p1=%v\n", p1)  //p1={pprof.cn 北京 18}
	fmt.Printf("p1=%#v\n", p1) //p1=main.person{name:"pprof.cn", city:"北京", age:18}
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
	var p2 = new(person)
	fmt.Printf("%T\n", p2)     //*main.person
	fmt.Printf("p2=%#v\n", p2) //p2=&main.person{name:"", city:"", age:0}

	p2.name = "测试"
	p2.age = 18
	p2.city = "北京"
	fmt.Printf("p2=%#v\n", p2) //p2=&main.person{name:"测试", city:"北京", age:18}
}

```

