# IO

## [io — 基本的 IO 接口 · Go语言标准库](https://books.studygolang.com/The-Golang-Standard-Library-by-Example/chapter01/01.1.html)

### Reader 接口

```go
package main

import (
	"fmt"
	"io"
	"os"
)

func ReadFrom(reader io.Reader, num int) ([]byte, error) {
	p := make([]byte, num)
	n, err := reader.Read(p)
	if n > 0 {
		return p[:n], err
	}
	return p, err
}

func main() {

	p, err := ReadFrom(os.Stdin, 4)
	if err != nil {
		fmt.Println("read error: ", err)
		return
	}
	fmt.Println(string(p))
}

```



```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func ReadFrom(reader io.Reader, num int) ([]byte, error) {
	p := make([]byte, num)
	n, err := reader.Read(p)
	if n > 0 {
		return p[:n], err
	}
	return p, err
}

func main() {

	data, err := ReadFrom(strings.NewReader("from string"), 10)
	if err != nil {
		panic("Error!")
	}
	fmt.Println(string(data))
}
```



### ReaderAt 接口

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	reader := strings.NewReader("Go语言中文网")
	p := make([]byte, 10)
	n, err := reader.ReadAt(p, 1)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%s, %d\n", p, n)
}

```

