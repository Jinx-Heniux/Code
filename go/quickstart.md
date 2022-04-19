# Quickstart

## Install Go

```shell
# Install Go on Ubuntu 20.04
# https://go.dev/doc/install
# https://www.digitalocean.com/community/tutorials/how-to-install-go-on-ubuntu-20-04


#################### Step 1 — Installing Go ####################

# Download Go: https://go.dev/dl/
curl -OL https://go.dev/dl/go1.18.1.linux-amd64.tar.gz

# To verify the integrity of the file you downloaded, 
# run the sha256sum command and pass it to the filename as an argument:
sha256sum go1.18.1.linux-amd64.tar.gz

# Remove any previous Go installation by deleting the /usr/local/go folder 
# (if it exists), then extract the archive you just downloaded into /usr/local, 
# creating a fresh Go tree in /usr/local/go:
# rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.1.linux-amd64.tar.gz
rm -rf /usr/local/go
sudo tar -C /usr/local -xvf go1.18.1.linux-amd64.tar.gz



#################### Step 2 — Setting Go Paths ####################

# Use your preferred editor to open .profile, 
# which is stored in your user’s home directory. 
# Here, we’ll use nano:
sudo nano ~/.profile
# Then, add the following information to the end of your file:
. . .
export PATH=$PATH:/usr/local/go/bin
# Next, refresh your profile by running the following command:
source ~/.profile
# After, check if you can execute go commands by running go version:
go version



#################### Step 3 — Testing Your Install ####################

# By default, the GOPATH variable, 
# which specifies the location of the workspace is set to $HOME/go. 
# To create the workspace directory type:
mkdir ~/go

# Inside the workspace create a new directory src/hello:
mkdir -p ~/go/src/example.jinx.com/hello # domain_name/project_name/

# When importing packages, you have to manage the dependencies 
# through the code’s own module. 
# You can do this by creating a go.mod file with the go mod init command:
cd ~/go/src/example.jinx.com/hello # domain_name/project_name/
go mod init example.jinx.com/hello

# and in that directory create a file named hello.go:
vim hello.go

# Test your code to check that it prints the Hello, World! greeting:
go run .


#################### Step 4 — Turning Your Go Code Into a Binary Executable ####################

# Navigate** to the ~/go/src/hello directory and 
# run go build to build the program:
cd ~/go/src/example.jinx.com/hello
go build
# The command above will build an executable file named hello.

# You can run the executable by simply executing the command below:
./hello
```



## Pointer

```go
package main

import "fmt"

func modify1(x int) {
	x = 100
}

func modify2(x *int) {
	*x = 100
}

func main() {

    	var name = "World"
	fmt.Println("Hello ", name)

	a := 10
	b := &a
	fmt.Printf("type of b: %T\n", b)
	fmt.Printf("type of a: %T\n", a)
	c := *b
	fmt.Printf("type of c: %T\n", c)
	fmt.Printf("value of c: %v\n", c)

	var p *string
	fmt.Println(&p)
	d := "Hello"
	p = &d
	fmt.Printf("address of p: %v\n", p)
	fmt.Printf("value of p: %v\n", *p)
	if p != nil {
		fmt.Println("not null")
	} else {
		fmt.Println("null")
	}

	e := 10
	modify1(e)
	fmt.Println(e)
	modify2(&e)
	fmt.Println(e)

}

// https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%8C%87%E9%92%88.html
```



## Scan

```go
package main

import "fmt"

func main() {
	var (
		name    string
		age     int
		married bool
	)
	fmt.Scan(&name, &age, &married)
	fmt.Printf("scanning result -> name:%s age:%d married:%t \n", name, age, married)
}

// https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/fmt.html
```



## 读取用户的输入

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

// https://www.coursera.org/learn/golang-getting-started/peer/f1Cly/module-2-activity-findian-go
// https://learnku.com/docs/the-way-to-go/121-reads-user-input/3661
// https://books.studygolang.com/The-Golang-Standard-Library-by-Example/chapter02/02.1.html
// https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/fmt.html
```



