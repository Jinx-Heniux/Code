# Array

## 数组Array · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%95%B0%E7%BB%84Array.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%95%B0%E7%BB%84Array.html)

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

}

```

