# Struct

## 结构体 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E7%BB%93%E6%9E%84%E4%BD%93.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E7%BB%93%E6%9E%84%E4%BD%93.html)

### 类型定义和类型别名的区别

```go
package main

import (
	"fmt"
)

//类型定义
type NewInt int

//类型别名
type MyInt = int

func main() {
	var a NewInt
	var b MyInt

	fmt.Printf("type of a: %T\n", a) //type of a:main.NewInt
	fmt.Printf("type of b: %T\n", b) //type of b:int
}

// 结果显示a的类型是main.NewInt，表示main包下定义的NewInt类型。
// b的类型是int。MyInt类型只会在代码中存在，编译完成时并不会有MyInt类型。

```



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
	// 结构体初始化
	var p1 person
	fmt.Printf("p1=%#v\n", p1)
	//p1=main.person{name:"", city:"", age:0}

	p1.name = "pprof.cn"
	p1.city = "北京"
	p1.age = 18
	fmt.Printf("p1=%v\n", p1) //p1={pprof.cn 北京 18}
	fmt.Printf("p1=%#v\n", p1)
	//p1=main.person{name:"pprof.cn", city:"北京", age:18}
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
	// 可以通过使用new关键字对结构体进行实例化，得到的是结构体的地址。
	var p2 = new(person)
	fmt.Printf("%T\n", p2)     //*main.person // p2是一个结构体指针
	fmt.Printf("p2=%#v\n", p2) //p2=&main.person{name:"", city:"", age:0}

	p2.name = "测试"
	p2.age = 18
	p2.city = "北京"
	fmt.Printf("p2=%#v\n", p2) //p2=&main.person{name:"测试", city:"北京", age:18}
}

```

### 实例化

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

	/* 取结构体的地址实例化 */
	p3 := &person{}
	// 使用&对结构体进行取地址操作相当于对该结构体类型进行了一次new实例化操作。
	fmt.Printf("%T\n", p3)     //*main.person
	fmt.Printf("p3=%#v\n", p3) //p3=&main.person{name:"", city:"", age:0}

	p3.name = "博客"
	// p3.name = "博客"其实在底层是(*p3).name = "博客"，这是Go语言帮我们实现的语法糖。
	p3.age = 30
	p3.city = "成都"
	fmt.Printf("p3=%#v\n", p3)
	//p3=&main.person{name:"博客", city:"成都", age:30}

	/* 使用键值对初始化 */
	// 使用键值对对结构体进行初始化时，键对应结构体的字段，值对应该字段的初始值。
	p5 := person{
		name: "pprof.cn",
		city: "北京",
		age:  18,
	}
	fmt.Printf("p5=%#v\n", p5) //p5=main.person{name:"pprof.cn", city:"北京", age:18}

	// 也可以对结构体指针进行键值对初始化
	p6 := &person{
		name: "pprof.cn",
		city: "北京",
		age:  18,
	}
	fmt.Printf("p6=%#v\n", p6) //p6=&main.person{name:"pprof.cn", city:"北京", age:18}

	p7 := &person{
		city: "北京",
	}
	fmt.Printf("p7=%#v\n", p7) //p7=&main.person{name:"", city:"北京", age:0}

	/* 使用值的列表初始化 */
	p8 := &person{
		"pprof.cn",
		"北京",
		18,
	}
	fmt.Printf("p8=%#v\n", p8) //p8=&main.person{name:"pprof.cn", city:"北京", age:18}
}

```

