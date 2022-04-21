# Array

## 数组Array · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%95%B0%E7%BB%84Array.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%95%B0%E7%BB%84Array.html)

一维数组

```go
package main

import "fmt"

var arr0 [5]int = [5]int{1, 2, 3}
var arr1 = [5]int{1, 2, 3, 4, 5}
var arr2 = [...]int{1, 2, 3, 4, 5, 6}
var str = [5]string{3: "Hello", 4: "Tom"}

func main() {

	a := [3]int{1, 2}
	b := [...]int{1, 2, 3}
	c := [5]int{0: 100, 4: 100}
	d := [...]struct {
		name string
		age  uint8
	}{
		// {name: "user1", age: 10},
		// {name: "user2", age: 20},
		{"user1", 10},
		{"user2", 20},
	}

	fmt.Println(arr0, arr1, arr2, str)
	fmt.Println(a, b, c, d)
}
```

多维数组

```go
package main

import "fmt"

var arr0 [5][3]int
var arr1 [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}

func main() {

	a := [2][3]int{{1, 2, 3}, {4, 5, 6}}
	b := [...][2]int{{1, 2}, {2, 3}, {3, 4}, {4, 5}} // 第 2 纬度不能用 "..."。
	fmt.Println(arr0, arr1)
	fmt.Println(a, b)

}

```

值拷贝行为会造成性能问题，通常会建议使用 slice，或数组指针。

```go
package main

import "fmt"

func test(x [2]int) {
	fmt.Printf("x: %p\n", &x)
	x[1] = 1000
}

func main() {

	a := [2]int{}
	fmt.Printf("a: %#p\n", &a)
	test(a)
	fmt.Printf("a: %p\n", &a)
	fmt.Println(a)

	fmt.Println(len(a), cap(a)) // 内置函数 len 和 cap 都返回数组长度 (元素数量)。

}

```

多维数组遍历

```go
package main

import "fmt"

func main() {

	var arr [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}

	for k1, v1 := range arr {
		for k2, v2 := range v1 {
			fmt.Printf("(%d,%d)=%d	", k1, k2, v2)
		}
		fmt.Println()
	}

}

```

数组拷贝和传参

```go
package main

import "fmt"

func printArr(arr *[5]int) {
	arr[0] = 10
	for k, v := range arr {
		fmt.Printf("%d -> %d\n", k, v)
	}
}

func main() {
	// arr1 := [5]int{}
	var arr1 [5]int
	printArr(&arr1)
	fmt.Println(arr1)

	arr2 := [...]int{1, 2, 3, 4, 5}
	printArr(&arr2)
	fmt.Println(arr2)
}

```
