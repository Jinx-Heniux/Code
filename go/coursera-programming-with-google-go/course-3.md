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

