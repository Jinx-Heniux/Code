# Coursera - Programming with Google Go

## module-2-activity-trunc-go

[https://www.coursera.org/learn/golang-getting-started/peer/01wGe/module-2-activity-trunc-go](https://www.coursera.org/learn/golang-getting-started/peer/01wGe/module-2-activity-trunc-go)

```go
package main

import "fmt"

func main() {
	var fp_num float32
	fmt.Println("Please enter a floating point number:")
	fmt.Scan(&fp_num)
	fmt.Printf("The truncated version of the given floating point number: %d", int(fp_num))
}
```

```go
package main
import "fmt"
func main(){

	for i := 0;i<2 ;i++ {
		fmt.Printf("Enter a Floating point number!\n")

	var number float64
	fmt.Scanln(&number)
	fmt.Println(int(number))

	}
	
}

```

## module-2-activity-findian-go

[https://www.coursera.org/learn/golang-getting-started/peer/f1Cly/module-2-activity-findian-go](https://www.coursera.org/learn/golang-getting-started/peer/f1Cly/module-2-activity-findian-go)

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

var inputReader *bufio.Reader
var input string
var err error

func main() {
	inputReader = bufio.NewReader(os.Stdin)
	fmt.Println("Please enter a string: ")
	input, err = inputReader.ReadString('\n')
	if err == nil {
		input = strings.TrimSpace(strings.ToLower(input))
		fmt.Printf("The input was: %s\n", input)
	} else {
		return
	}
	// input_lower := strings.TrimSpace(strings.ToLower(input))
	fmt.Printf("The input was: ##%s##\n", input)
	fmt.Println(strings.HasPrefix(input, "i"))
	fmt.Println(strings.HasSuffix(input, "n"))
	fmt.Println(strings.Contains(input, "a"))
	fmt.Println(strings.HasPrefix(input, "i") && strings.HasSuffix(input, "n") && strings.Contains(input, "a"))
	if strings.HasPrefix(input, "i") && strings.HasSuffix(input, "n") && strings.Contains(input, "a") {
		fmt.Println("Found!")
	} else {
		fmt.Println("Not Found!")
	}
}

/*
import (
	"fmt"
	"strings"
)

func main() {
	var input_str string
	fmt.Println("Please enter a string:")
	fmt.Scan(&input_str)
	fmt.Println(input_str)
	input_str_lower := strings.ToLower(input_str)
	fmt.Println(input_str_lower)
	if strings.HasPrefix(input_str_lower, "i") && strings.HasSuffix(input_str_lower, "n") && strings.Contains(input_str_lower, "a") {
		fmt.Println("Found!")
	} else {
		fmt.Println("Not Found!")
	}
}
*/

```



```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

var inputReader *bufio.Reader
var input string
var err error

func main() {
	inputReader = bufio.NewReader(os.Stdin)
	fmt.Println("Please enter a string: ")
	input, err = inputReader.ReadString('\n')
	if err == nil {
		input = strings.TrimSpace(strings.ToLower(input))
	}

	if strings.HasPrefix(input, "i") && strings.HasSuffix(input, "n") && strings.Contains(input, "a") {
		fmt.Println("Found!")
	} else {
		fmt.Println("Not Found!")
	}
}

```



```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

func main() {
	fmt.Println("enter the string= ")
	scanner := bufio.NewScanner(os.Stdin)
	scanner.Scan()
	stri := scanner.Text()
	//fmt.Println("you have entered", stri)
	lower := strings.ToLower(stri)
	formattedString := strings.ReplaceAll(lower, " ", "")
	fmt.Println(formattedString)

	if strings.Contains(formattedString, "a") {
		if strings.HasPrefix(formattedString, "i") {
			if strings.HasSuffix(formattedString, "n") {
				fmt.Println("Found!")
				os.Exit(0)
			}
		}

	}
	fmt.Println("Not Found!")
}

```



## module-3-activity-slice-go

[https://www.coursera.org/learn/golang-getting-started/peer/sLPZg/module-3-activity-slice-go](https://www.coursera.org/learn/golang-getting-started/peer/sLPZg/module-3-activity-slice-go)

```go
package main

import (
	"fmt"
	"sort"
	"strconv"
)

func main() {

	s := make([]int, 0, 3)

	for {
		fmt.Println("Enter 'X' to exit!")
		fmt.Printf("Please enter a number:\n")
		var x string
		fmt.Scanln(&x)
		fmt.Printf("x->%v(%T)\n", x, x)
		if x == "X" {
			// fmt.Println("I'm here.")
			break
		} else {
			i, err := strconv.Atoi(x)
			if err != nil {
				panic(err)
			}
			fmt.Printf("i->%d(%T)\n", i, i)
			s = append(s, i)
		}

	}
	fmt.Printf("s->%v(%T)\n", s, s)
	sort.Ints(s)
	fmt.Printf("s->%v(%T)\n", s, s)

}

```

