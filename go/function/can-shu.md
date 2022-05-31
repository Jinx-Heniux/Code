# 参数

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





