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



### ReadDir 函数

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

func main() {
	dir := os.Args[1]
	fmt.Println(dir)
	listAll(dir, 0)
}

func listAll(path string, curHier int) {
	fileInfos, err := ioutil.ReadDir(path)
	if err != nil {
		fmt.Println(err)
		return
	}

	for _, info := range fileInfos {
		if info.IsDir() {
			for tmpHier := curHier; tmpHier > 0; tmpHier-- {
				fmt.Printf("|\t")
			}
			fmt.Println(info.Name(), "\\")
			listAll(path+"/"+info.Name(), curHier+1)
		} else {
			for tmpHier := curHier; tmpHier > 0; tmpHier-- {
				fmt.Printf("|\t")
			}
			fmt.Println(info.Name())
		}
	}
}

```

