---
description: 闭包
---

# Closure

## [闭包、递归 · Go语言中文文档](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E9%97%AD%E5%8C%85%E9%80%92%E5%BD%92.html)



```go
package main

import (
	"fmt"
)

func a() func() int {
	i := 0
	b := func() int {
		i++
		fmt.Println(i)
		return i
	}
	return b
}

func main() {
	c := a()
	c()
	c()
	c()

	a() //不会输出i

	c2 := a()
	c2()
	c2()
	c2()
}

/*
1
2
3
1
2
3
*/


```



### 闭包对象指针

```go
package main

import "fmt"

func test() func() {
	x := 100
	fmt.Printf("x (%p) = %d\n", &x, x)

	return func() {
		fmt.Printf("x (%p) = %d\n", &x, x)
	}
}

func main() {
	f := test()
	fmt.Printf("%T \n", f)
	f()

	fmt.Println()
	f2 := test()
	f2()
}
/*
x (0xc0000ba000) = 100
func() 
x (0xc0000ba000) = 100

x (0xc0000ba020) = 100
x (0xc0000ba020) = 100
*/
```



### 外部引用函数参数局部变量

```go
package main

import "fmt"

// 外部引用函数参数局部变量
func add(base int) func(int) int {
	return func(i int) int {
		base += i
		return base
	}
}

func main() {
	tmp1 := add(10)
	fmt.Println(tmp1(1), tmp1(2))
	// 此时tmp1和tmp2不是一个实体了
	tmp2 := add(100)
	fmt.Println(tmp2(1), tmp2(2))
}

/*
11 13
101 103
*/

```



### 返回2个闭包

```go
package main

import "fmt"

// 返回2个函数类型的返回值
func test01(base int) (func(int) int, func(int) int) {
	// 定义2个函数，并返回
	// 相加
	add := func(i int) int {
		base += i
		return base
	}
	// 相减
	sub := func(i int) int {
		base -= i
		return base
	}
	// 返回
	return add, sub
}

func main() {
	f1, f2 := test01(10)
	// base一直是没有消
	fmt.Println(f1(1), f2(2))
	// 此时base是9
	fmt.Println(f1(3), f2(4))
}

```



### 递归函数

### 数字阶乘 factorial

```go
package main

import "fmt"

func factorial(i int) int {
	if i <= 1 {
		return 1
	}
	return i * factorial(i-1)
}

func main() {
	// var i int = 7 // Factorial of 7 is 5040
	// i := 2 // Factorial of 2 is 2
	i := 3 // Factorial of 3 is 6
	fmt.Printf("Factorial of %d is %d\n", i, factorial(i))
}

```



### 斐波那契数列(Fibonacci)

```go
package main

import "fmt"

func fibonaci(i int) int {
	if i == 0 {
		return 0
	}
	if i == 1 {
		return 1
	}
	return fibonaci(i-1) + fibonaci(i-2)
}

func main() {
	var i int
	for i = 0; i < 10; i++ {
		fmt.Printf("i=%d -> %d\n", i, fibonaci(i))
	}
}

```

