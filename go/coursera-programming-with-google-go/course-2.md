# Course 2

## module-1-activity-bubble-sort-program

[https://www.coursera.org/learn/golang-functions-methods/peer/UxoIW/module-1-activity-bubble-sort-program/](https://www.coursera.org/learn/golang-functions-methods/peer/UxoIW/module-1-activity-bubble-sort-program/)

### by me

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
				Swap(slice, i, k)
			}
		}
	}
}

func main() {

	reader := bufio.NewReader(os.Stdin)
	fmt.Println("Please enter up to 10 integers separated by a space (e.g. 4 56 3 63 6 -325 35 0 -342 -4):")

	inputStr, err := reader.ReadString('\n')
	if err == nil {
		inputStr = strings.TrimSpace(inputStr)
	} else {
		return
	}

	fields := strings.Fields(inputStr)
	if len(fields) > 10 {
		fmt.Println("Input Error: please enter up to 10 integers!")
		os.Exit(1)
	}

	inputIntSlice := make([]int, 0, 10)

	for field := range fields {
		i, err := strconv.Atoi(fields[field])
		if err != nil {
			fmt.Println("Invalid Input!")
			os.Exit(2)
		}
		inputIntSlice = append(inputIntSlice, i)
	}

	// fmt.Println("Original integers:")
	// fmt.Printf("    -> %v\n", inputIntSlice)
	BubbleSort(inputIntSlice)
	// fmt.Println("Sorted integers:")
	// fmt.Printf("    -> %v\n", inputIntSlice)

	fmt.Printf("Sorted integers: %v\n", inputIntSlice)
}

// by me
```

### by Doug Donohoe

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func main() {
	in := bufio.NewReader(os.Stdin)

	// prompt user for numbers
	fmt.Printf("Please enter up to 10 numbers (separated by spaces): ")
	numsString, err := in.ReadString('\n')
	if err != nil {
		fmt.Printf("Unable to read input: %s", err)
		os.Exit(1)
	}

	// convert to slice of strings
	numsSlice := strings.Split(strings.TrimSpace(numsString), " ")

	// convert to integers
	var nums []int
	for i, num := range numsSlice {
		// no more than 10 elements
		if i == 10 {
			break
		}
		nums = append(nums, StringToInt(num))
	}

	// output info + sort
	fmt.Printf("Input received: %v\n", nums)
	BubbleSort(nums)
	fmt.Printf("After bubble sort: %v\n", nums)

}

func StringToInt(num string) int {
	i, err := strconv.Atoi(num)
	if err != nil {
		fmt.Printf("Error converting '%s' to integer (using 0 instead): %s\n", num, err)
	}
	return i
}

func BubbleSort(slice []int) {
	length := len(slice)
	for i := 0; i < length-1; i++ {
		for j := 0; j < length-i-1; j++ {
			if slice[j] > slice[j+1] {
				Swap(slice, j)
			}
		}
	}
}

func Swap(slice []int, pos int) {
	slice[pos], slice[pos+1] = slice[pos+1], slice[pos]
}


```

### by Ansh joshi

```go
package main

import (
	"fmt"
)

func main() {
	n := 10
	var arr [10]int
	fmt.Println("Enter numbers \n")
	var temp int
	for i := 0; i < n; i++ {
		fmt.Scan(&temp)
		arr[i] = temp
	}

	fmt.Println(arr)
	Bubble(&arr, n)
	fmt.Println("Sorted array is :")
	fmt.Println(arr)
}

func Bubble(arr *[10]int, n int) {
	//i is for number of swaps steps to take
	for i := 0; i < n; i++ {
		for j := 0; j < n-i-1; j++ {
			if arr[j] > arr[j+1] {
				swap(&arr[j], &arr[j+1])
			}
		}
	}
}

func swap(a *int, b *int) {
	var temp int
	temp = *a
	*a = *b
	*b = temp
}

```



## module-2-activity

[https://www.coursera.org/learn/golang-functions-methods/peer/qKrnv/module-2-activity/](https://www.coursera.org/learn/golang-functions-methods/peer/qKrnv/module-2-activity/)

### by me

```go
package main

import (
	"fmt"
)

func GenDisplaceFn(a, v0, s0 float64) func(float64) float64 {
	return func(t float64) float64 {
		return ((a * t * t) / 2.0) + (v0 * t) + s0
	}

}

func main() {

	var (
		a  float64
		v0 float64
		s0 float64
		t  float64
	)
	fmt.Println("Please enter values for acceleration, initial velocity, and initial displacement:")
	fmt.Println("(e.g. 10 2 1):")
	fmt.Scan(&a, &v0, &s0)
	fmt.Println("Please enter a value for time:")
	fmt.Println("(e.g. 3):")
	fmt.Scan(&t)
	fmt.Printf("Your input: \n\tacceleration: %v\n\tinitial velocity: %v\n\tinitial displacement: %v\n\ttime: %v\n", a, v0, s0, t)

	fn := GenDisplaceFn(a, v0, s0)
	fmt.Printf("The result for computing the displacement after %v seconds: %v\n", t, fn(t))

}

```

