---
description: 切片
---

# Slice

## [6、slice和map · 语雀](https://www.yuque.com/aceld/mo95lb/ovmzgh)



```go
package main

import "fmt"

func main() {
	var numbers = make([]int, 3, 5)
	numbers[2] = 100
	fmt.Printf("address of number: %p | numbers: %v\n", &numbers, numbers)
	printSlice(numbers)
}

func printSlice(x []int) {
	x[2] = 200
	fmt.Printf("address of x: %p | x: %v\n", &x, x)
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

/*
address of number: 0xc0000ac018 | numbers: [0 0 100]
address of x: 0xc0000ac048 | x: [0 0 200]
len=3 cap=5 slice=[0 0 200]
*/


```



### 空切片 nil

```go
package main

import "fmt"

func main() {
	var numbers []int
	fmt.Printf("address of number: %p | numbers: %v\n", &numbers, numbers)
	printSlice(numbers)

	if numbers == nil {
		fmt.Println("slice is nil")
	}
}

func printSlice(x []int) {
	fmt.Printf("address of x: %p | x: %v\n", &x, x)
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

/*
address of number: 0xc00000c030 | numbers: []
address of x: 0xc00000c048 | x: []
len=0 cap=0 slice=[]
slice is nil
*/


```



### 切片截取

```go
package main

import "fmt"

func main() {
	/* 创建切片 */
	numbers := []int{0, 1, 2, 3, 4, 5, 6, 7, 8}
	printSlice(numbers)

	/* 打印原始切片 */
	fmt.Println("numbers ==", numbers)

	/* 打印子切片从索引1(包含) 到索引4(不包含)*/
	fmt.Println("numbers[1:4] ==", numbers[1:4])

	/* 默认下限为 0*/
	fmt.Println("numbers[:3] ==", numbers[:3])

	/* 默认上限为 len(s)*/
	fmt.Println("numbers[4:] ==", numbers[4:])

	numbers1 := make([]int, 0, 5)
	printSlice(numbers1)

	/* 打印子切片从索引  0(包含) 到索引 2(不包含) */
	number2 := numbers[:2]
	printSlice(number2)

	/* 打印子切片从索引 2(包含) 到索引 5(不包含) */
	number3 := numbers[2:5]
	printSlice(number3)

}

func printSlice(x []int) {
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

/*
len=9 cap=9 slice=[0 1 2 3 4 5 6 7 8]
numbers == [0 1 2 3 4 5 6 7 8]
numbers[1:4] == [1 2 3]
numbers[:3] == [0 1 2]
numbers[4:] == [4 5 6 7 8]
len=0 cap=5 slice=[]
len=2 cap=9 slice=[0 1]
len=3 cap=7 slice=[2 3 4]
*/


```



### append() 和 copy() 函数

```go
package main

import "fmt"

func main() {
	var numbers []int
	printSlice(numbers)

	/* 允许追加空切片 */
	numbers = append(numbers, 0)
	printSlice(numbers)

	/* 向切片添加一个元素 */
	numbers = append(numbers, 1)
	printSlice(numbers)

	/* 同时添加多个元素 */
	numbers = append(numbers, 2, 3, 4)
	printSlice(numbers)

	/* 创建切片 numbers1 是之前切片的两倍容量*/
	numbers1 := make([]int, len(numbers), (cap(numbers))*2)

	/* 拷贝 numbers 的内容到 numbers1 */
	copy(numbers1, numbers)
	printSlice(numbers1)
}

func printSlice(x []int) {
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

/*
len=0 cap=0 slice=[]
len=1 cap=1 slice=[0]
len=2 cap=2 slice=[0 1]
len=5 cap=6 slice=[0 1 2 3 4]
len=5 cap=12 slice=[0 1 2 3 4]
*/


```

