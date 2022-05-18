# Course 2

## module-1-activity-bubble-sort-program

[https://www.coursera.org/learn/golang-functions-methods/peer/UxoIW/module-1-activity-bubble-sort-program/](https://www.coursera.org/learn/golang-functions-methods/peer/UxoIW/module-1-activity-bubble-sort-program/)



```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func Swap(slice []int, i int, j int) {
	slice[i], slice[j] = slice[j], slice[i]
}

func BubbleSort(slice []int) {
	for i := 0; i < len(slice)-1; i++ {
		for k := i + 1; k < len(slice); k++ {
			if slice[i] > slice[k] {
				// slice[i], slice[k] = slice[k], slice[i]
				Swap(slice, i, k)
			}
		}
	}
}

func main() {

	reader := bufio.NewReader(os.Stdin) // 从标准输入生成读对象
	fmt.Println("Please enter 10 integers separated by a space:")
	inputStr, err := reader.ReadString('\n') // 读到换行
	// input, err = inputReader.ReadString('\n')
	if err == nil {
		// input = strings.TrimSpace(strings.ToLower(input))
		// fmt.Printf("The input was: %s\n", input)
		inputStr = strings.TrimSpace(inputStr)
		fmt.Printf("inputStr -> %#v\n", inputStr)
	} else {
		return
	}
	// inputStr = strings.TrimSpace(inputStr)
	// fmt.Printf("inputStr -> %#v\n", inputStr)
	// fmt.Printf("%v\n", inputStr)

	// fields := strings.Split(inputStr, " ")
	fields := strings.Fields(inputStr)

	if len(fields) > 10 {
		fmt.Println("Input Error: please enter up to 10 integers!")
		os.Exit(1)
	}
	fmt.Printf("fields -> %#v\n", fields)

	inputIntSlice := make([]int, 0, 10)
	fmt.Printf("inputIntSlice -> %#v\n", inputIntSlice)

	for field := range fields {
		fmt.Printf("field %d -> %#v\n", field, fields[field])
		i, err := strconv.Atoi(fields[field])
		if err != nil {
			fmt.Println("Invalid Input!")
			os.Exit(2)
		}
		inputIntSlice = append(inputIntSlice, i)
	}
	fmt.Printf("inputIntSlice -> %#v\n", inputIntSlice)

	// lst := []int{2, 5, 13, 7, 21, 1, 3, 2, 0, 9, 6, 7}
	// lst := []int{-10, 2, 5, 13, 7, -1, 21, 1, 3, 2, 0, -5, 9, 6, 7}
	// fmt.Printf("排序前：%v\n", lst)
	BubbleSort(inputIntSlice)
	// fmt.Printf("排序后：%v\n", lst)
	fmt.Printf("Printing the integers out in sorted order, from least to greatest -> %#v\n", inputIntSlice)
}
// by me
```

