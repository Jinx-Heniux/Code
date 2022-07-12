# Course 3

## [module-2-activity](https://www.coursera.org/learn/golang-concurrency/peer/RAJ0V/module-2-activity)

### by me

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var Wait sync.WaitGroup
var Counter int = 0

func main() {

	for routine := 1; routine <= 2; routine++ {

		Wait.Add(1)
		go Routine(routine)
	}

	Wait.Wait()
	fmt.Printf("Final Counter: %d\n", Counter)
}

func Routine(id int) {

	for count := 0; count < 2; count++ {

		value := Counter
		time.Sleep(1 * time.Nanosecond)
		value++
		Counter = value
	}

	Wait.Done()
}

/*
If you run the program multiple times, the result of the variable "Final Counter"
could be 4 or 2 as output below:
Final Counter: 4
Final Counter: 2

The reason is that there are two goroutines that execute concurrently at the same time
and increament the same globle variable Counter.


A race condition is a shared data that will be accessed and modified
by two or more threads or processes.
In Golang, a race condition occurs when multiple goroutines try to
access and modify the same data such as a variable or a struct.
For instance, if one goroutine tires to increase an integer and another thread tries to read it.

*/

```



### by others

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"time"
)

var x int

/*
This programs shows how to raise a race condition.
There is a shared variable called 'x' and two goroutines.
Each goroutine make an increment of x, so the output should be:
	Value 1
	Value 2

But there is not like this. The output is
	Value 2
	Value

You can see why in the comments in the code below
*/
func main() {
	x = 0

	//This goroutine emulates a long operation after updating the shared variable
	go func(i *int) {
		//the shared variable here is set to 1
		*i++
		time.Sleep(500)
		//Here, the variable should be 1, but is not, is set to 2 by the second goroutine
		fmt.Println("Value", *i)
	}(&x)

	//Since this goroutine is executed after the increment in the first goroutine but before the sleep(500)
	go func(i *int) {
		//The variable here is 1, so it is set to 2
		*i++
		//Prints 2
		fmt.Println("Value", *i)
	}(&x)

	bufio.NewReader(os.Stdin).ReadBytes('\n')

}

```



### by Sabbir Miah

```go
package main

import (
	"fmt"
	"time"
)

func changeVal(x *int) {
	*x = 5
	return
}
func getValue() int {
	var v int
	go changeVal(&v)
	time.Sleep(200 * time.Nanosecond)
	return v
}

func main() {
	for {
		time.Sleep(1000)
		fmt.Println(getValue())

	}
}

```



## [module-3-activity](https://www.coursera.org/learn/golang-concurrency/peer/meeiu/module-3-activity/)

### by me

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
	"strings"
	"sync"
	"unsafe"
)

/*
func SortIntSlice(is []int) []int {
	fmt.Printf("%#v, %p, %d,%d\n", is, is, len(is), cap(is))
	sort.Ints(is)
	fmt.Printf("%#v, %p, %d,%d\n", is, is, len(is), cap(is))
	return is
}
*/

func SortIntSlice(wg *sync.WaitGroup, is *[]int) {
	fmt.Printf("%#v, %p, %d,%d\n", is, is, len(*is), cap(*is))
	sort.Ints(*is)
	fmt.Printf("%#v, %p, %d,%d\n", is, is, len(*is), cap(*is))
	wg.Done()
}

/*
func SortIntSlice(is []int) {
	fmt.Printf("%#v, %p, %d,%d\n", is, is, len(is), cap(is))
	sort.Ints(is)
	fmt.Printf("%#v, %p, %d,%d\n", is, is, len(is), cap(is))
}
*/
func splitSlice(arr []int, num int64) [][]int {
	max := int64(len(arr))
	// fmt.Println(max)
	if max < num {
		return nil
	}
	var sliceOfSlices = make([][]int, 0)
	quantity := max / num
	end := int64(0)
	// fmt.Println(quantity, end)
	for i := int64(1); i <= num; i++ {
		qu := i * quantity
		if i != num {
			sliceOfSlices = append(sliceOfSlices, arr[i-1+end:qu])
		} else {
			sliceOfSlices = append(sliceOfSlices, arr[i-1+end:])
		}
		end = qu - i
	}
	return sliceOfSlices
}

func mergSlices(soss [][]int) []int {
	slice := make([]int, 0)
	for i := 0; i < len(soss); i++ {
		slice = append(slice, soss[i]...)
	}
	return slice
}

func main() {

	inputReader := bufio.NewReader(os.Stdin)
	fmt.Println("Please enter a series of integers (separated by a space, press enter to finish the input):")
	fmt.Print("(e.g. 4 635 56 6 -3 4 0 -445 -453 45 45 35 4535645 -5345 0 4534 -894 98 0 3487 -253)\n>")
	inputStr, err := inputReader.ReadString('\n')
	if err != nil {
		fmt.Println("Invalid Input!")
		os.Exit(2)
	}
	fields := strings.Fields(inputStr)
	// fmt.Printf("fields -> %#v\n", fields)
	inputIntSlice := make([]int, 0)
	// fmt.Printf("inputIntSlice -> %#v\n", inputIntSlice)

	for field := range fields {
		// fmt.Printf("field %d -> %#v\n", field, fields[field])
		i, err := strconv.Atoi(fields[field])
		if err != nil {
			fmt.Println("Invalid Input!")
			os.Exit(2)
		}
		inputIntSlice = append(inputIntSlice, i)
	}

	// fmt.Printf("inputIntSlice -> %#v\n", inputIntSlice)
	// sort.Ints(inputIntSlice)
	// SortIntSlice(inputIntSlice)
	// fmt.Printf("inputIntSlice -> %#v\n", inputIntSlice)

	sliceOfSubslices := splitSlice(inputIntSlice, 4)
	fmt.Printf("%v, %p, %d, %d\n", sliceOfSubslices, sliceOfSubslices, len(sliceOfSubslices), cap(sliceOfSubslices))

	var wg sync.WaitGroup
	// for slice := range results {
	for i := 0; i < len(sliceOfSubslices); i++ {
		wg.Add(1)
		fmt.Printf("partition %d: %#v, %p, %p, %d,%d\n", i, sliceOfSubslices[i], sliceOfSubslices[i], &sliceOfSubslices[i], len(sliceOfSubslices[i]), cap(sliceOfSubslices[i]))
		// *ptr := results[i]
		// ptr := (*[]int)(unsafe.Pointer(&results[i]))
		// SortIntSlice(ptr)
		// go sort.Ints(results[slice])
		// fmt.Printf("ending goroutine for partition %d: %v\n", slice, results[slice])
		// go SortIntSlice(results[i])
		// temp := make([]int, 0)
		go SortIntSlice(&wg, (*[]int)(unsafe.Pointer(&sliceOfSubslices[i])))
	}
	// time.Sleep(time.Second)
	wg.Wait()
	fmt.Println(sliceOfSubslices)

	mergedSlice := mergSlices(sliceOfSubslices)
	fmt.Println(mergedSlice)
	sort.Ints(mergedSlice)
	fmt.Println(mergedSlice)

}

/*
Please enter a series of integers (separated by a space, press enter to finish the input):
(e.g. 4 635 56 6 -3 4 0 -445 -453 45 45 35 4535645 -5345 0 4534 -894 98 0 3487 -253)
>4 635 56 6 -3 4 0 -445 -453 45 45 35 4535645 -5345 0 4534 -894 98 0 3487 -253
[[4 635 56 6 -3] [4 0 -445 -453 45] [45 35 4535645 -5345 0] [4534 -894 98 0 3487 -253]], 0xc000192000, 4, 4
partition 0: []int{4, 635, 56, 6, -3}, 0xc00018c000, 0xc000192000, 5,32
partition 1: []int{4, 0, -445, -453, 45}, 0xc00018c028, 0xc000192018, 5,27
partition 2: []int{45, 35, 4535645, -5345, 0}, 0xc00018c050, 0xc000192030, 5,22
partition 3: []int{4534, -894, 98, 0, 3487, -253}, 0xc00018c078, 0xc000192048, 6,17
&[]int{4534, -894, 98, 0, 3487, -253}, 0xc000192048, 6,17
&[]int{-894, -253, 0, 98, 3487, 4534}, 0xc000192048, 6,17
&[]int{4, 635, 56, 6, -3}, 0xc000192000, 5,32
&[]int{-3, 4, 6, 56, 635}, 0xc000192000, 5,32
&[]int{4, 0, -445, -453, 45}, 0xc000192018, 5,27
&[]int{-453, -445, 0, 4, 45}, 0xc000192018, 5,27
&[]int{45, 35, 4535645, -5345, 0}, 0xc000192030, 5,22
&[]int{-5345, 0, 35, 45, 4535645}, 0xc000192030, 5,22
[[-3 4 6 56 635] [-453 -445 0 4 45] [-5345 0 35 45 4535645] [-894 -253 0 98 3487 4534]]
[-3 4 6 56 635 -453 -445 0 4 45 -5345 0 35 45 4535645 -894 -253 0 98 3487 4534]
[-5345 -894 -453 -445 -253 -3 0 0 0 4 4 6 35 45 45 56 98 635 3487 4534 4535645]
*/

```



### by me

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
	"strings"
	"sync"
	"unsafe"
)

// This sorts a slice of integers.
func SortIntSlice(wg *sync.WaitGroup, is *[]int) {
	sort.Ints(*is)
	wg.Done()
}

// This splits a slice of integer into num partitions (num sub-slices of integers).
func splitSlice(arr []int, num int64) [][]int {
	max := int64(len(arr))
	if max < num {
		return nil
	}
	var sliceOfSlices = make([][]int, 0)
	quantity := max / num
	end := int64(0)
	for i := int64(1); i <= num; i++ {
		qu := i * quantity
		if i != num {
			sliceOfSlices = append(sliceOfSlices, arr[i-1+end:qu])
		} else {
			sliceOfSlices = append(sliceOfSlices, arr[i-1+end:])
		}
		end = qu - i
	}
	return sliceOfSlices
}

// This merges a slice of integer slices into a single slice of integers.
func mergeSlices(soss [][]int) []int {
	slice := make([]int, 0)
	for i := 0; i < len(soss); i++ {
		slice = append(slice, soss[i]...)
	}
	return slice
}

func main() {

	// receive user input
	inputReader := bufio.NewReader(os.Stdin)
	fmt.Println("Please enter a series of integers (separated by a space, press enter to finish the input):")
	fmt.Print("(e.g. 4 635 56 6 -3 4 0 -445 -453 45 45 35 4535645 -5345 0 4534 -894 98 0 3487 -253)\n>")
	inputStr, err := inputReader.ReadString('\n')
	if err != nil {
		fmt.Println("Invalid Input!")
		os.Exit(2)
	}
	fields := strings.Fields(inputStr)
	inputIntSlice := make([]int, 0)

	// convert user input from string to integer
	for field := range fields {
		i, err := strconv.Atoi(fields[field])
		if err != nil {
			fmt.Println("Invalid Input!")
			os.Exit(2)
		}
		inputIntSlice = append(inputIntSlice, i)
	}

	// split slices
	sliceOfSubslices := splitSlice(inputIntSlice, 4)

	// create one goroutine for one subslice to sort it concurrently
	var wg sync.WaitGroup
	for i := 0; i < len(sliceOfSubslices); i++ {
		wg.Add(1)
		fmt.Printf("partition %d: %v\n", i, sliceOfSubslices[i])
		go SortIntSlice(&wg, (*[]int)(unsafe.Pointer(&sliceOfSubslices[i])))
	}

	wg.Wait()

	fmt.Println("before merging: ", sliceOfSubslices)

	mergedSlice := mergeSlices(sliceOfSubslices)
	sort.Ints(mergedSlice)
	fmt.Println("after merging: ", mergedSlice)

}

/*
Please enter a series of integers (separated by a space, press enter to finish the input):
(e.g. 4 635 56 6 -3 4 0 -445 -453 45 45 35 4535645 -5345 0 4534 -894 98 0 3487 -253)
>4 635 56 6 -3 4 0 -445 -453 45 45 35 4535645 -5345 0 4534 -894 98 0 3487 -253
partition 0: [4 635 56 6 -3]
partition 1: [4 0 -445 -453 45]
partition 2: [45 35 4535645 -5345 0]
partition 3: [4534 -894 98 0 3487 -253]
before merging:  [[-3 4 6 56 635] [-453 -445 0 4 45] [-5345 0 35 45 4535645] [-894 -253 0 98 3487 4534]]
after merging:  [-5345 -894 -453 -445 -253 -3 0 0 0 4 4 6 35 45 45 56 98 635 3487 4534 4535645]
*/

```



### by Anand raja

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	var n, e int
	fmt.Printf("How many numbers you want to enter (min=4) : ")

	fmt.Scanf("%d", &n)
	if n < 4 {
		panic("Min 4 number of ints required")
	}
	nums := make([]int, 0)
	fmt.Printf("Enter number and hit return/enter\n")

	for i := 0; i < n; i++ {
		fmt.Scanf("%d", &e)
		nums = append(nums, e)
	}
	fmt.Printf("After input => %v \n", nums)

	var res []int
	ch := make(chan []int)
	step := n / 4

	for i := 0; i < n; i += step {
		go sortNums(nums[i:i+step], ch)
	}

	for i := 0; i < 4; i++ {
		rs1 := <-ch
		res = merge(res, rs1)
	}

	sort.Ints(res)
	fmt.Printf("Final Sorted output %v\n", res)
}

func sortNums(nums []int, ch chan []int) {
	fmt.Printf("Input => %v \n", nums)
	sort.Ints(nums)
	fmt.Printf("Sorted => %v \n", nums)
	ch <- nums
}

func merge(left, right []int) []int {

	length, i, j := len(left)+len(right), 0, 0
	slice := make([]int, length, length)

	for k := 0; k < length; k++ {
		if i > len(left)-1 && j <= len(right)-1 {
			slice[k] = right[j]
			j++
		} else if j > len(right)-1 && i <= len(left)-1 {
			slice[k] = left[i]
			i++
		} else if left[i] < right[j] {
			slice[k] = left[i]
			i++
		} else {
			slice[k] = right[j]
			j++
		}
	}
	return slice
}

/*
How many numbers you want to enter (min=4) : 12
Enter number and hit return/enter
45 356 76 4 3 746 0 5 45 657 23 76
After input => [45 356 76 4 3 746 0 5 45 657 23 76]
Input => [657 23 76]
Input => [4 3 746]
Input => [0 5 45]
Sorted => [3 4 746]
Sorted => [23 76 657]
Input => [45 356 76]
Sorted => [45 76 356]
Sorted => [0 5 45]
Final Sorted output [0 3 4 5 23 45 45 76 76 356 657 746]
*/

```



### by Xuyan Xu

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
	"strings"
	"sync"
)

func merge(left, right []int) (result []int) {
	result = make([]int, len(left)+len(right))

	i := 0
	for len(left) > 0 && len(right) > 0 {
		if left[0] < right[0] {
			result[i] = left[0]
			left = left[1:]
		} else {
			result[i] = right[0]
			right = right[1:]
		}
		i++
	}

	for j := 0; j < len(left); j++ {
		result[i] = left[j]
		i++
	}
	for j := 0; j < len(right); j++ {
		result[i] = right[j]
		i++
	}

	return
}

func sortInts(a []int, wg *sync.WaitGroup) {
	fmt.Println(a) // Each goroutine which sorts ¼ of the array should print the subarray that it will sort
	sort.Ints(a)
	if wg != nil {
		wg.Done()
	}
}

func sortBy4Routines(a []int) {
	var wg sync.WaitGroup

	n := len(a)
	m := n / 4
	carry := n % 4
	buckets := []int{m, 0, m, 0, m, 0, m, 0} //[low, high, low, high, ...]

	for i := 0; i < carry; i++ {
		buckets[i*2] += 1
	}

	start := 0
	for i := 0; i < 4; i++ {
		buckets[i*2+1] = start + buckets[i*2]
		buckets[i*2] = start
		start = buckets[i*2+1]
	}

	wg.Add(4)
	for i := 0; i < 4; i++ {
		go sortInts(a[buckets[i*2]:buckets[i*2+1]], &wg)
	}
	wg.Wait()

	x := merge(a[buckets[0]:buckets[1]], a[buckets[2]:buckets[3]])
	y := merge(a[buckets[4]:buckets[5]], a[buckets[6]:buckets[7]])

	fmt.Println(merge(x, y)) //When sorting is complete, the main goroutine should print the entire sorted list.
}

func main() {

	fmt.Println("Please input a series of integer with space delimiter:") //The program should prompt the user to input a series of integers
	scanner := bufio.NewScanner(os.Stdin)

	var input string
	if scanner.Scan() {
		input = scanner.Text()
	}

	// Convert input to integers
	fields := strings.Fields(input)
	n := len(fields)
	nums := make([]int, n)
	for i := 0; i < n; i++ {
		num, err := strconv.Atoi(fields[i])
		if err != nil {
			fmt.Println(err)
			os.Exit(-1)
		}
		nums[i] = num
	}

	// if the input len is less than 4, we sort them directly.
	// Otherwise we create 4 goroutines to sort each part and merge them together.
	if n >= 4 {
		sortBy4Routines(nums)
	} else {
		sortInts(nums, nil)
		fmt.Println(nums)
	}
}

/*
Please input a series of integer with space delimiter:
45 356 76 4 3 746 0 5 45 657 23 76
[657 23 76]
[45 356 76]
[4 3 746]
[0 5 45]
[0 3 4 5 23 45 45 76 76 356 657 746]
*/

```



### by Jason Tucker

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
	"sync"
)

func sortSlice(s *[]int, wg *sync.WaitGroup) {
	fmt.Printf("sortSlice() sorting %v\n", *s)
	sort.Ints(*s)
	wg.Done()
}

func merge(s1 []int, s2 []int, c chan []int) {
	var m []int
	l := len(s1)
	if l < len(s2) {
		l = len(s2)
	}
	for len(s1) > 0 && len(s2) > 0 {
		if s1[0] <= s2[0] {
			m = append(m, s1[0])
			s1 = s1[1:]
		} else {
			m = append(m, s2[0])
			s2 = s2[1:]
		}
	}
	for i := 0; i < len(s1); i++ {
		m = append(m, s1[i])
	}
	for i := 0; i < len(s2); i++ {
		m = append(m, s2[i])
	}
	c <- m
	return
}

func main() {
	var wg sync.WaitGroup
	var a []int
	var split1 int

	fmt.Println("Enter integers, 1 per line (CTRL-D to end):")
	scanner := bufio.NewScanner(os.Stdin)
	scanner.Split(bufio.ScanLines)
	for scanner.Scan() {
		i, _ := strconv.Atoi(scanner.Text())
		a = append(a, i)
	}
	if len(a)%4 < 3 {
		split1 = (len(a) / 4)
	} else {
		split1 = (len(a) / 4) + 1
	}
	split2 := split1 * 2
	split3 := split1 * 3

	s1 := make([]int, split1)
	s2 := make([]int, split1)
	s3 := make([]int, split1)
	s4 := make([]int, split1)
	s1 = a[0:split1]
	s2 = a[split1:split2]
	s3 = a[split2:split3]
	s4 = a[split3:len(a)]
	wg.Add(4)
	go sortSlice(&s1, &wg)
	go sortSlice(&s2, &wg)
	go sortSlice(&s3, &wg)
	go sortSlice(&s4, &wg)
	wg.Wait()
	c := make(chan []int)
	go merge(s1, s2, c)
	go merge(s3, s4, c)
	m1 := <-c
	m2 := <-c
	go merge(m1, m2, c)
	m := <-c
	fmt.Printf("merged set: %v\n", m)

}

/*
Enter integers, 1 per line (CTRL-D to end):
34
56
47
587
57
456
45
46
5
456
6
456
sortSlice() sorting [456 6 456]
sortSlice() sorting [45 46 5]
sortSlice() sorting [34 56 47]
sortSlice() sorting [587 57 456]
merged set: [5 6 34 45 46 47 56 57 456 456 456 587]
*/

```



## module-4-activity

### by me

```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

var wg sync.WaitGroup
var host = make(chan int, 2)

type Chopstick struct{ sync.Mutex }

type Philosopher struct {
	id              int
	leftCS, rightCS *Chopstick
}

func (p Philosopher) eat() {
	defer wg.Done()
	for i := 0; i < 3; i++ {

		host <- 1

		rand.Seed(time.Now().UnixNano())
		flag := rand.Intn(2)
		if flag == 1 {
			p.rightCS.Lock()
			p.leftCS.Lock()
		} else {
			p.leftCS.Lock()
			p.rightCS.Lock()
		}
		fmt.Printf("starting to eat %d for %d time(s)\n", p.id, i+1)
		time.Sleep(time.Second * time.Duration(flag+1))
		fmt.Printf("finishing to eat %d for %d time(s)\n", p.id, i+1)
		if flag == 1 {
			p.rightCS.Unlock()
			p.leftCS.Unlock()
		} else {
			p.leftCS.Unlock()
			p.rightCS.Unlock()
		}
		<-host
	}
}

func main() {
	Chopsticks := make([]*Chopstick, 5)

	for i := 0; i < 5; i++ {
		Chopsticks[i] = new(Chopstick)
	}

	Philosophers := make([]*Philosopher, 5)

	for i := 0; i < 5; i++ {
		Philosophers[i] = &Philosopher{
			i + 1,
			Chopsticks[i],
			Chopsticks[(i+1)%5],
		}
	}

	for i := 0; i < 5; i++ {
		wg.Add(1)
		go Philosophers[i].eat()
	}

	wg.Wait()
}

/*
starting to eat 5 for 1 time(s)
starting to eat 2 for 1 time(s)
finishing to eat 5 for 1 time(s)
finishing to eat 2 for 1 time(s)
starting to eat 3 for 1 time(s)
finishing to eat 3 for 1 time(s)
starting to eat 1 for 1 time(s)
starting to eat 4 for 1 time(s)
finishing to eat 4 for 1 time(s)
finishing to eat 1 for 1 time(s)
starting to eat 5 for 2 time(s)
starting to eat 2 for 2 time(s)
finishing to eat 5 for 2 time(s)
finishing to eat 2 for 2 time(s)
starting to eat 3 for 2 time(s)
finishing to eat 3 for 2 time(s)
starting to eat 1 for 2 time(s)
starting to eat 4 for 2 time(s)
finishing to eat 4 for 2 time(s)
finishing to eat 1 for 2 time(s)
starting to eat 2 for 3 time(s)
starting to eat 5 for 3 time(s)
finishing to eat 2 for 3 time(s)
starting to eat 3 for 3 time(s)
finishing to eat 5 for 3 time(s)
finishing to eat 3 for 3 time(s)
starting to eat 1 for 3 time(s)
starting to eat 4 for 3 time(s)
finishing to eat 4 for 3 time(s)
finishing to eat 1 for 3 time(s)
*/

```



