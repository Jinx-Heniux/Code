# ioutil

## [ioutil — 方便的 IO 操作函数集 · Go语言标准库](https://books.studygolang.com/The-Golang-Standard-Library-by-Example/chapter01/01.2.html)

### ReadAll 函数

```go
package main

import (
	"fmt"
	"io/ioutil"
	"strings"
)

func main() {
	reader := strings.NewReader("Go语言中文网")
	data, err := ioutil.ReadAll(reader)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%#v\n", data)
	fmt.Println(string(data))
}

```

