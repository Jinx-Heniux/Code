# Function

## [函数定义](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E5%87%BD%E6%95%B0%E5%AE%9A%E4%B9%89.html)



```go
package main

import "fmt"

func test0(x, y int, s string) (int, string) {
	// 类型相同的相邻参数，参数类型可合并。 多返回值必须用括号。
	n := x + y
	return n, fmt.Sprintf(s, n)
}

func main() {

	i, s := test0(1, 2, "haha")
	fmt.Printf("i -> %#v, s -> %#v\n", i, s)
	fmt.Printf("s -> %v (%T) ", s, s)
	/*
		i -> 3, s -> "haha%!(EXTRA int=3)"
		s -> haha%!(EXTRA int=3) (string)

		格式化错误
		实参太多：%!(EXTRA type=value)
			Printf("hi", "guys"):      hi%!(EXTRA string=guys)
			https://books.studygolang.com/The-Golang-Standard-Library-by-Example/chapter01/01.3.html?h=Sprintf
	*/
}

```



### 函数作为参数

### 函数类型

```go
package main

import "fmt"

func test(fn func() int) int {
	return fn()
}

// 定义函数类型。
type FormatFunc func(s string, x, y int) string

func format(fn FormatFunc, s string, x, y int) string {
	return fn(s, x, y)
}

func main() {
	s1 := test(func() int { return 100 }) // 直接将匿名函数当参数。

	s2 := format(func(s string, x, y int) string {
		return fmt.Sprintf(s, x, y)
	}, "%d, %d", 10, 20)

	fmt.Printf("s1 -> %#v | %v | %d \n", s1, s1, s1)
	fmt.Printf("s2 -> %#v | %v | %s \n", s2, s2, s2)
	/*
		s1 -> 100 | 100 | 100
		s2 -> "10, 20" | 10, 20 | 10, 20
	*/
}

```



## [参数 · Go语言中文文档](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E5%8F%82%E6%95%B0.html)



### 引用传递

```go
package main

import (
	"fmt"
)

/* 定义相互交换值的函数 */
func swap(x, y *int) {
	var temp int

	temp = *x /* 保存 x 的值 */
	*x = *y   /* 将 y 值赋给 x */
	*y = temp /* 将 temp 值赋给 y*/

}

func main() {
	var a, b int = 1, 2
	/*
	   调用 swap() 函数
	   &a 指向 a 指针，a 变量的地址
	   &b 指向 b 指针，b 变量的地址
	*/
	swap(&a, &b)

	fmt.Printf("a: %d b: %d", a, b)
}

```



### 可变参数

```go
package main

import (
	"fmt"
)

func test(s string, n ...int) string {
	var x int
	for _, i := range n {
		x += i
	}

	return fmt.Sprintf(s, x)
}

func main() {
	println(test("sum: %d", 1, 2, 3, 4, 5))
}

```



### 展开slice

```go
package main

import (
	"fmt"
)

func test(s string, n ...int) string {
	var x int
	for _, i := range n {
		x += i
	}

	return fmt.Sprintf(s, x)
}

func main() {
	s := []int{1, 2, 3}
	res := test("sum: %d", s...) // slice... 展开slice
	println(res)
}

```



## [返回值 · Go语言中文文档](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E8%BF%94%E5%9B%9E%E5%80%BC.html)



### “裸”返回

```go
package main

import (
	"fmt"
)

func add(a, b int) (c int) {
	c = a + b
	return
}

func calc(a, b int) (sum int, avg int) {
	sum = a + b
	avg = (a + b) / 2

	return
}

func main() {
	var a, b int = 1, 2
	c := add(a, b)
	sum, avg := calc(a, b)
	fmt.Println(a, b, c, sum, avg)

	e := 3.0
	f := 2.0
	fmt.Printf("e -> %#v | %T \n", e, e)
	g := e / f
	fmt.Printf("g -> %#v | %T | %v | %.2f\n", g, g, g, g)

}

```



### 多返回值

```go
package main

func test() (int, int) {
	return 1, 2
}

func main() {
	// s := make([]int, 2)
	// s = test()   // Error: multiple-value test() in single-value context

	x, _ := test()
	println(x)

	y, z := test()
	println(y, z)
}

```



```go
package main

func test() (int, int) {
	return 1, 2
}

func add(x, y int) int {
	return x + y
}

func sum(n ...int) int {
	var x int
	for _, i := range n {
		x += i
	}

	return x
}

func main() {
	println(add(test()))
	println(sum(test()))
}

```



### 命名返回参数

```go
package main

func add(x, y int) (z int) {
	z = x + y
	return
}

func main() {
	println(add(1, 2))
}

```



```go
package main

func add(x, y int) (z int) {
	{ // 不能在一个级别，引发 "z redeclared in this block" 错误。
		var z = x + y
		// return   // Error: z is shadowed during return
		return z // 必须显式返回。
	}
}

func main() {
	println(add(1, 2))
}

```

