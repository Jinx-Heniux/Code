# Pointer

## 指针 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%8C%87%E9%92%88.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%8C%87%E9%92%88.html)

```go
package main

import "fmt"

func modify1(x int) {
	x = 100
}

func modify2(x *int) {
	*x = 100
}

func main() {

    	var name = "World"
	fmt.Println("Hello ", name)

	a := 10
	b := &a
	fmt.Printf("type of b: %T\n", b)
	fmt.Printf("type of a: %T\n", a)
	c := *b
	fmt.Printf("type of c: %T\n", c)
	fmt.Printf("value of c: %v\n", c)

	var p *string
	fmt.Println(&p)
	d := "Hello"
	p = &d
	fmt.Printf("address of p: %v\n", p)
	fmt.Printf("value of p: %v\n", *p)
	if p != nil {
		fmt.Println("not null")
	} else {
		fmt.Println("null")
	}

	e := 10
	modify1(e)
	fmt.Println(e)
	modify2(&e)
	fmt.Println(e)

}


```



