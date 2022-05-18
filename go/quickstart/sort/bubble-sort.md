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

