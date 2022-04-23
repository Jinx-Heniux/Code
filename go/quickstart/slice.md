# Slice

## 切片Slice · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%88%87%E7%89%87Slice.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%88%87%E7%89%87Slice.html)

### 创建切片

```go
package main

import (
	"fmt"
)

func main() {
	//1.声明切片
	var s1 []int
	if s1 == nil {
		fmt.Println("是空")
	} else {
		fmt.Println("不是空")
	}
	fmt.Println()
	// 2.:=
	s2 := []int{}
	// 3.make()
	var s3 []int = make([]int, 0)
	fmt.Println(s1, s2, s3)
	fmt.Println()
	// 4.初始化赋值
	var s4 []int = make([]int, 0, 0)
	fmt.Println(s4)
	fmt.Println()
	s5 := []int{1, 2, 3}
	fmt.Println(s5)
	fmt.Println()
	// 5.从数组切片
	arr := [5]int{1, 2, 3, 4, 5}
	var s6 []int
	// 前包后不包
	s6 = arr[1:4]
	fmt.Println(s6)
}

```

### 切片初始化

```go
package main

import (
	"fmt"
)

var arr = [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
var slice0 []int = arr[2:8]
var slice1 []int = arr[0:6]        //可以简写为 var slice []int = arr[:end]
var slice2 []int = arr[5:10]       //可以简写为 var slice[]int = arr[start:]
var slice3 []int = arr[0:len(arr)] //var slice []int = arr[:]
var slice4 = arr[:len(arr)-1]      //去掉切片的最后一个元素
func main() {
	fmt.Printf("全局变量: arr %v\n", arr)
	fmt.Printf("全局变量: slice0 %v\n", slice0)
	fmt.Printf("全局变量: slice1 %v\n", slice1)
	fmt.Printf("全局变量: slice2 %v\n", slice2)
	fmt.Printf("全局变量: slice3 %v\n", slice3)
	fmt.Printf("全局变量: slice4 %v\n", slice4)
	fmt.Printf("-----------------------------------\n")
	arr2 := [...]int{9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
	slice5 := arr[2:8]
	slice6 := arr[0:6]         //可以简写为 slice := arr[:end]
	slice7 := arr[5:10]        //可以简写为 slice := arr[start:]
	slice8 := arr[0:len(arr)]  //slice := arr[:]
	slice9 := arr[:len(arr)-1] //去掉切片的最后一个元素
	fmt.Printf("局部变量:  arr2 %v\n", arr2)
	fmt.Printf("局部变量:  slice5 %v\n", slice5)
	fmt.Printf("局部变量:  slice6 %v\n", slice6)
	fmt.Printf("局部变量:  slice7 %v\n", slice7)
	fmt.Printf("局部变量:  slice8 %v\n", slice8)
	fmt.Printf("局部变量:  slice9 %v\n", slice9)
}

```

### 通过make来创建切片

```go
package main

import (
	"fmt"
)

var slice0 []int = make([]int, 10)
var slice1 = make([]int, 10)
var slice2 = make([]int, 10, 10)

func main() {
	fmt.Printf("make全局slice0 : %v\n", slice0)
	fmt.Printf("make全局slice1 : %v\n", slice1)
	fmt.Printf("make全局slice2 : %v\n", slice2)
	fmt.Println("--------------------------------------")
	slice3 := make([]int, 10)
	slice4 := make([]int, 10)
	slice5 := make([]int, 10, 10)
	fmt.Printf("make局部slice3 : %v\n", slice3)
	fmt.Printf("make局部slice4 : %v\n", slice4)
	fmt.Printf("make局部slice5 : %v\n", slice5)
}

```

### 读写操作实际目标是底层数组

```go
package main

import (
	"fmt"
)

func main() {
	data := [...]int{0, 1, 2, 3, 4, 5}

	s := data[2:4]
	s[0] += 100
	s[1] += 200

	fmt.Println(s)
	fmt.Println(data)
}
```

