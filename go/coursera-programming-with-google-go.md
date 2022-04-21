# Coursera - Programming with Google Go

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

