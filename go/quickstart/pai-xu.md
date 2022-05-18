---
description: 排序
---

# Sort

## 字符串数组排序 · Go语言中文文档

[https://www.topgoer.com/%E5%85%B6%E4%BB%96/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%95%B0%E7%BB%84%E6%8E%92%E5%BA%8F.html](https://www.topgoer.com/%E5%85%B6%E4%BB%96/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%95%B0%E7%BB%84%E6%8E%92%E5%BA%8F.html)



```go
package main

import (
	"fmt"
	"sort"
	"strconv"
)

func main() {

	// myList := []string{"1", "10", "11", "2", "3", "4", "5", "6", "7", "8", "9"}
	myList := []string{"1", "10", "11", "6", "7", "8", "9", "2", "3", "4", "5"}

	fmt.Printf("Before: %v (%T) (%p)\n", myList, myList, &myList)

	temp1 := sort.StringSlice(myList)
	fmt.Printf("After sort.StringSlice: %v (%T) (%p)\n", myList, myList, &myList)
	fmt.Println(temp1)
	// fmt.Println(myList)

	sort.Strings(myList)
	// fmt.Println(temp2)
	fmt.Printf("After sort.Strings: %v (%T) (%p)\n", myList, myList, &myList)
	// fmt.Println(myList)

	// Pass in our list and a func to compare values
	sort.Slice(myList, func(i, j int) bool {
		numA, _ := strconv.Atoi(myList[i])
		numB, _ := strconv.Atoi(myList[j])
		return numA < numB
	})

	// fmt.Printf("After: %v\n", myList)
	fmt.Printf("After: %v (%T) (%p)\n", myList, myList, &myList)
}

/*
Before: [1 10 11 6 7 8 9 2 3 4 5] ([]string) (0xc00000c030)
After sort.StringSlice: [1 10 11 6 7 8 9 2 3 4 5] ([]string) (0xc00000c030)
[1 10 11 6 7 8 9 2 3 4 5]
After sort.Strings: [1 10 11 2 3 4 5 6 7 8 9] ([]string) (0xc00000c030)
After: [1 2 3 4 5 6 7 8 9 10 11] ([]string) (0xc00000c030)
*/

```

