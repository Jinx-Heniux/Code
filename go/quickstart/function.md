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

