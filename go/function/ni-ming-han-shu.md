# 匿名函数

## [匿名函数 · Go语言中文文档](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0.html)

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	getSqrt := func(a float64) float64 {
		return math.Sqrt(a)
	}
	fmt.Println(getSqrt(4))
}

```



```go
package main

func main() {
	// --- function variable ---
	fn := func() { println("Hello, World!") }
	fn()

	// --- function collection ---
	fns := [](func(x int) int){
		func(x int) int { return x + 1 },
		func(x int) int { return x + 2 },
	}
	println(fns[0](100))

	// --- function as field ---
	d := struct {
		fn func() string
	}{
		fn: func() string { return "Hello, World!" },
	}
	println(d.fn())

	// --- channel of function ---
	fc := make(chan func() string, 2)
	fc <- func() string { return "Hello, World!" }
	println((<-fc)())
}

/*
Hello, World!
101
Hello, World!
Hello, World!
*/

```

