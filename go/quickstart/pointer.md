# Pointer

## 指针 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%8C%87%E9%92%88.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%8C%87%E9%92%88.html)

### 指针地址和指针类型

```go
package main

import (
	"fmt"
)

func main() {
	a := 10
	b := &a
	fmt.Printf("a:%d ptr:%p\n", a, &a) // a:10 ptr:0xc00001a078
	fmt.Printf("b:%p type:%T\n", b, b) // b:0xc00001a078 type:*int
	fmt.Println(&b)                    // 0xc00000e018
}
```



### 指针取值

```go
package main

import (
	"fmt"
)

func main() {
	//指针取值
	a := 10
	b := &a // 取变量a的地址，将指针保存到b中
	fmt.Printf("type of b:%T\n", b)
	c := *b // 指针取值（根据指针去内存取值）
	fmt.Printf("type of c:%T\n", c)
	fmt.Printf("value of c:%v\n", c)
}
```

### 指针传值

```go
package main

import (
	"fmt"
)

func modify1(x int) {
	x = 100
}

func modify2(x *int) {
	*x = 100
}

func main() {
	a := 10
	modify1(a)
	fmt.Println(a) // 10
	modify2(&a)
	fmt.Println(a) // 100
}
```

### 空指针

```go
package main

import (
	"fmt"
)

func main() {

	var p *string
	fmt.Printf("address of p: %v\n", &p)
	fmt.Printf("type of p: %T\n", p)
	fmt.Printf("value of p: %v\n", p)
	if p != nil {
		fmt.Println("not null")
	} else {
		fmt.Println("null")
	}
	//fmt.Printf("value of p: %s\n", *p)
	// panic: runtime error: invalid memory address or nil pointer dereference
	d := "Hello"
	p = &d
	fmt.Println("++++++")
	fmt.Printf("address of p: %v\n", &p)
	fmt.Printf("type of p: %T\n", p)
	fmt.Printf("value of p: %v\n", p)
	fmt.Printf("value of *p: %s\n", *p)

	if p != nil {
		fmt.Println("not null")
	} else {
		fmt.Println("null")
	}

}

/*
address of p: 0xc0000bc018
type of p: *string
value of p: <nil>
null
++++++
address of p: 0xc0000bc018
type of p: *string
value of p: 0xc000096230
value of *p: Hello
not null
*/


```



