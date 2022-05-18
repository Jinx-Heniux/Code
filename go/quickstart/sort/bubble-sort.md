---
description: 冒泡排序
---

# Bubble Sort

## Golang实现冒泡排序算法（Bubble Sort） - 完美代码

[https://www.perfcode.com/p/golang-bubble-sort.html](https://www.perfcode.com/p/golang-bubble-sort.html)



```go
package main

import (
	"fmt"
)

func bubbleSort(arr []int) []int {
	swapped := true
	for swapped {
		swapped = false
		for i := 0; i < len(arr)-1; i++ {
			if arr[i+1] < arr[i] {
				arr[i+1], arr[i] = arr[i], arr[i+1]
				swapped = true
			}
		}
	}
	return arr
}

func main() {
	lst := []int{-10, 2, 5, 13, 7, -1, 21, 1, 3, 2, 0, -5, 9, 6, 7}
	fmt.Printf("排序前：%v\n", lst)
	fmt.Printf("排序后：%v\n", bubbleSort(lst))
}

```



## Go 冒泡排序 ( Bubble Sort ) - SegmentFault 思否

[https://segmentfault.com/a/1190000015654916](https://segmentfault.com/a/1190000015654916)



```go
package main

import (
	"fmt"
)

// 冒泡
func Bubble_Sort(slice []int /* 待排序的切片 */, order bool /* true : 正序; false : 倒序; */) {

	/*
	   1. 本循环代表 依次与其后所有数作比较的索引
	   2. 由于最后一个数不用比较，所以 len(slice) - 1
	*/
	for i := 0; i < len(slice)-1; i++ {

		// 本循环代表 外层将要与哪一位作比较
		for k := i + 1; k < len(slice); k++ {

			if (slice[i] > slice[k] && order) /* 从大到小 */ || (slice[i] <= slice[k] && !order) /* 从小到大 */ {

				slice[i], slice[k] = slice[k], slice[i]

			}
		}
	}
}

func main() {
	lst := []int{-10, 2, 5, 13, 7, -1, 21, 1, 3, 2, 0, -5, 9, 6, 7}
	fmt.Printf("排序前：%v\n", lst)
	Bubble_Sort(lst, true)
	fmt.Printf("排序后：%v\n", lst)
}

```

