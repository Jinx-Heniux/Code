# Function

## 函数定义

[https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E5%87%BD%E6%95%B0%E5%AE%9A%E4%B9%89.html](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E5%87%BD%E6%95%B0%E5%AE%9A%E4%B9%89.html)



```go
package main

import "fmt"

func test0(x, y int, s string) (int, string) {
	// 类型相同的相邻参数，参数类型可合并。 多返回值必须用括号。
	n := x + y
	return n, fmt.Sprintf(s, n)
}

func test(fn func() int) int {
	return fn()
}

// 定义函数类型。
type FormatFunc func(s string, x, y int) string

func format(fn FormatFunc, s string, x, y int) string {
	return fn(s, x, y)
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

	s1 := test(func() int { return 100 }) // 直接将匿名函数当参数。

	s2 := format(func(s string, x, y int) string {
		return fmt.Sprintf(s, x, y)
	}, "%d, %d", 10, 20)

	println(s1, s2) // 100 10, 20
}

```

