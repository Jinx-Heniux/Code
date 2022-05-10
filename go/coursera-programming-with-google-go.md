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
				// panic(err)
				continue
			}
			fmt.Printf("i->%d(%T)\n", i, i)
			s = append(s, i)
		}

	}
	fmt.Printf("s->%v(%T)\n", s, s)
	sort.Ints(s)
	fmt.Printf("s->%v(%T)\n", s, s)

}
// by me
```



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
		// fmt.Println("Enter 'X' to exit!")
		fmt.Printf("Please enter a number (Enter 'X' to exit)):\n")
		var x string
		fmt.Scanln(&x)
		if x == "X" {
			break
		} else {
			i, err := strconv.Atoi(x)
			if err != nil {
				fmt.Println("Invalid Input!")
				continue
			}
			s = append(s, i)
			sort.Ints(s)
			fmt.Printf("%v\n", s)
		}

	}
	// sort.Ints(s)
	// fmt.Printf("User input numbers in sorted order: %v\n", s)

}

// by me
```



```go
package main

import (
	"bufio"
	"fmt"
	"log"
	"os"
	"regexp"
	"sort"
	"strconv"
	"strings"
)

type UserPrompt int

const (
	ListSize            int    = 3
	MaxUserPromptLength int    = 40
	QuitCode            string = "X"
)

const (
	ReadStringValue UserPrompt = iota
	ShowStringValue
	RegexFound
	RegexNotFound
)

func GetUserPromptPadded(up UserPrompt, str string, strFmt string) string {
	var paddedStr = str + strings.Repeat(" ", (MaxUserPromptLength-len(str))) + strFmt
	if !(up == ReadStringValue) {
		paddedStr = paddedStr + "\n"
	}
	return paddedStr
}

func GetUserPrompt(up UserPrompt) string {
	var strReturnVal string
	switch up {
	case ReadStringValue:
		strReturnVal = GetUserPromptPadded(up, "Please enter an integer ('X' to Exit):", "")
	case RegexFound:
		strReturnVal = GetUserPromptPadded(up, "Valid Input!", "")
	case RegexNotFound:
		strReturnVal = GetUserPromptPadded(up, "Invalid Input!", "")
	}
	return strReturnVal
}

func main() {
	var intSlice = make([]int, ListSize)

	re := regexp.MustCompile(`^\d+$`)
	scanner := bufio.NewScanner(os.Stdin)
	if err := scanner.Err(); err != nil {
		log.Println(err)
	}
	fmt.Printf(GetUserPrompt(ReadStringValue))
	for scanner.Scan() {
		var strVal string = scanner.Text()
		if strVal == QuitCode {
			os.Exit(0)
		}
		match := re.Match([]byte(strVal))
		intVal, err := strconv.Atoi(strVal)
		if match == false || err != nil {
			println(GetUserPrompt(RegexNotFound))
			fmt.Printf(GetUserPrompt(ReadStringValue))
			continue
		}
		intSlice = append(intSlice, intVal)
		sort.Sort(sort.IntSlice(intSlice))
		var result []int = intSlice[ListSize:len(intSlice)]
		fmt.Println(result)
		fmt.Printf(GetUserPrompt(ReadStringValue))
	}
}
// by pavani battula
```



## module-4-activity-makejson-go

[https://www.coursera.org/learn/golang-getting-started/peer/0xyF8/module-4-activity-makejson-go/](https://www.coursera.org/learn/golang-getting-started/peer/0xyF8/module-4-activity-makejson-go/)



```go
package main

import (
	"bufio"
	"encoding/json"
	"fmt"
	"os"
	"strings"
)

func main() {

	// receive user input
	inputReader := bufio.NewReader(os.Stdin)
	fmt.Println("Please entre a name: ")
	name, err := inputReader.ReadString('\n')
	if err == nil {
		name = strings.TrimSpace(name)
	} else {
		fmt.Println("Something is wrong, Please start the program again!")
		return
	}
	fmt.Println("Please entre an address: ")
	address, err := inputReader.ReadString('\n')
	if err == nil {
		address = strings.TrimSpace(address)
	} else {
		fmt.Println("Something is wrong, Please start the program again!")
		return
	}

	// create a map for user input
	userInfo := map[string]string{
		"name":    name,
		"address": address,
	}

	// fmt.Printf("userInfo -> %#v\n", userInfo)

	// JSON object from the map
	userInfoJson, err := json.Marshal(userInfo)
	if err != nil {
		fmt.Println("Something is wrong, Please start the program again!")
		return
	}
	// fmt.Printf("userInfo in JSON -> %#v\n", userInfoJson)
	fmt.Printf("userInfo in JSON -> %v\n", string(userInfoJson))

}

// by me
```



```go
package main

import (
	"bufio"
	"encoding/json"
	"fmt"
	"os"
	"strings"
)

func main() {
	var name string
	var address string

	fmt.Printf("Enter a name: \n")
	stdioIn := bufio.NewReader(os.Stdin)
	name, _ = stdioIn.ReadString('\n')
	name = strings.TrimSpace(name)

	fmt.Printf("\nEnter an address: \n")
	stdioIn = bufio.NewReader(os.Stdin)
	address, _ = stdioIn.ReadString('\n')
	address = strings.TrimSpace(address)

	nameAddress := make(map[string]string)

	nameAddress["name"] = name
	nameAddress["address"] = address

	var marshalled []byte
	marshalled, _ = json.Marshal(nameAddress)
	fmt.Println(string(marshalled))
}

```



## final-course-activity-read-go

[https://www.coursera.org/learn/golang-getting-started/peer/CHgQd/final-course-activity-read-go/](https://www.coursera.org/learn/golang-getting-started/peer/CHgQd/final-course-activity-read-go/)



```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
	"strings"
)

type name struct {
	fname string
	lname string
}

func main() {

	// receive user input
	inputReader := bufio.NewReader(os.Stdin)
	fmt.Println("Please enter a file name (e.g. /home/names.txt): ")
	filename, err := inputReader.ReadString('\n')
	if err == nil {
		filename = strings.TrimSpace(filename)
	} else {
		fmt.Println("Something is wrong, Please start the program again!")
		return
	}

	// filename := "/home/zhs2si/go/src/hello/names.txt"

	nameSlice := make([]name, 0, 20)

	/*
		// 读文件
		data, err := ioutil.ReadFile(filename)
		if err != nil {
			fmt.Println(err)
			return
		}
		fmt.Printf("data -> %#v\n", data)
		fmt.Printf("data -> %#v\n", string(data))
	*/

	file, err := os.Open(filename)
	if err != nil {
		fmt.Println(err)
		return
	}

	defer file.Close()

	reader := bufio.NewReader(file)
	for {
		line, _, err := reader.ReadLine()
		if err == io.EOF {
			break
		}
		if err != nil {
			return
		}
		// fmt.Println(string(line))

		fields := strings.Split(string(line), " ")
		// fmt.Printf("fields -> %#v\n", fields)
		/*
			for i := 0; i < len(fields); i++ {
				// fmt.Printf("firstname: %v, lastname: %v\n", fields[0], fields[1])
				name := name{
					fname: fields[0],
					lname: fields[1],
				}

			}
		*/
		n := name{
			fname: fields[0],
			lname: fields[1],
		}

		// fmt.Printf("name: %#v\n", n)
		nameSlice = append(nameSlice, n)

	}
	// fmt.Printf("name: %#v\n", nameSlice)

	for _, value := range nameSlice {
		fmt.Printf("first name: %v, last name: %v\n", value.fname, value.lname)
	}
}

// by me
```



```go
package main

import (
	"bufio"
	"fmt"
	"io/ioutil"
	"os"
	"strings"
)

type Name struct {
	first string
	last  string
}

type People []Name

func (people *People) collectDataFromFile(fileName string) {
	bytes, err := ioutil.ReadFile(fileName)
	if err != nil {
		fmt.Println("Error: unable to read file: ", err)
		return
	}

	scanner := bufio.NewScanner(strings.NewReader(string(bytes)))
	for scanner.Scan() {
		var name Name
		line := strings.Split(scanner.Text(), " ")
		name.first = line[0]
		name.last = line[1]

		*people = append(*people, name)
	}
}

func main() {
	var people People

	fmt.Printf("Enter file name: ")
	scanner := bufio.NewScanner(os.Stdin)
	scanner.Scan()
	if err := scanner.Err(); err != nil {
		fmt.Println("Error: ", err)
	}

	people.collectDataFromFile(scanner.Text())
	for _, name := range people {
		fmt.Printf("first_name: %v, \tlast_name: %v \n", name.first, name.last)
	}
}

```

