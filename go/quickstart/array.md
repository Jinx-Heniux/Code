# Array

## 数组Array · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%95%B0%E7%BB%84Array.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%95%B0%E7%BB%84Array.html)

### 一维数组

```go
package main

import "fmt"

// 全局
var arr0 [5]int = [5]int{1, 2, 3}
var arr1 = [5]int{1, 2, 3, 4, 5}
var arr2 = [...]int{1, 2, 3, 4, 5, 6}
var str = [5]string{3: "Hello", 4: "Tom"}
var arr [5]int // [0 0 0 0 0]

func main() {

	// 局部
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
	// [1 2 3 0 0] [1 2 3 4 5] [1 2 3 4 5 6] [   Hello Tom]
	fmt.Println(a, b, c, d)
	// [1 2 0] [1 2 3] [100 0 0 0 100] [{user1 10} {user2 20}]
}

```

### 多维数组

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

求数组所有元素之和

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func sumArr(arr [10]int) int {
	var sum int
	for i := 0; i < len(arr); i++ {
		sum += arr[i]
	}
	return sum
}

func main() {
	// 若想做一个真正的随机数，要种子
	// seed()种子默认是1
	//rand.Seed(1)
	rand.Seed(time.Now().Unix())

	var b [10]int
	for i := 0; i < len(b); i++ {
		// 产生一个0到1000随机数
		b[i] = rand.Intn(1000)
		fmt.Println(b[i])
	}
	sum := sumArr(b)
	fmt.Printf("sum=%d\n", sum)
}

```

找出数组中和为给定值的两个元素的下标，例如数组\[1,3,5,8,7]，找出两个元素之和等于8的下标分别是（0，4）和（1，2）

```go
package main

import (
	"fmt"
)

func myTest(arr [5]int, target int) {
	for i := 0; i < len(arr); i++ {
		other := target - arr[i]
		for j := i + 1; j < len(arr); j++ {
			if arr[j] == other {
				fmt.Printf("(%d,%d)\n", i, j)
			}
		}
	}

}

func main() {
	b := [5]int{1, 3, 5, 8, 7}
	myTest(b, 8)
}

```
