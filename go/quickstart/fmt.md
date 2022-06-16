# fmt

## [fmt · Go语言中文文档](https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/fmt.html)

### Print

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Print("在终端打印该信息。")
	name := "枯藤"
	fmt.Printf("我是：%s\n", name)
	fmt.Println("在终端打印单独一行显示")
}

// Print系列函数会将内容输出到系统的标准输出，
// 区别在于Print函数直接输出内容，
// Printf函数支持格式化输出字符串，
// Println函数会在输出内容的结尾添加一个换行符。
```

### Fprint

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	// 向标准输出写入内容
	fmt.Fprintln(os.Stdout, "向标准输出写入内容")
	fileObj, err := os.OpenFile("./xx.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)
	if err != nil {
		fmt.Println("打开文件出错, err:", err)
		return
	}
	name := "枯藤"
	// 向打开的文件句柄中写入内容
	fmt.Fprintf(fileObj, "往文件中写如信息：%s", name)
}
```

### Sprint

```go
package main

import (
	"fmt"
)

func main() {
	s1 := fmt.Sprint("枯藤\n")
	name := "枯藤"
	age := 18
	s2 := fmt.Sprintf("name:%s,age:%d\n", name, age)
	s3 := fmt.Sprintln("枯藤")
	fmt.Printf("type of s1: %T \n", s1)
	fmt.Printf("type of s2: %T \n", s2)
	fmt.Printf("type of s3: %T \n", s3)
	fmt.Println(s1, s2, s3)
}
```

### Scan

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


// Scan从标准输入扫描文本，读取由空白符分隔的值保存到传递给本函数的参数中，换行符视为空白符。
// 本函数返回成功扫描的数据个数和遇到的任何错误。
// 如果读取的数据个数比提供的参数少，会返回一个错误报告原因。
// fmt.Scan从标准输入中扫描用户输入的数据，将以空白符分隔的数据分别存入指定的参数。

```

### Scanf

```go
package main

import (
	"fmt"
)

func main() {
	var (
		name    string
		age     int
		married bool
	)
	fmt.Scanf("1:%s 2:%d 3:%t", &name, &age, &married)
	fmt.Printf("扫描结果 name:%s age:%d married:%t \n", name, age, married)
}

// 输入： 1:枯藤 2:18 3:false

// Scanf从标准输入扫描文本，
// 根据format参数指定的格式去读取由空白符分隔的值保存到传递给本函数的参数中。
// 本函数返回成功扫描的数据个数和遇到的任何错误。
// fmt.Scanf不同于fmt.Scan简单的以空格作为输入数据的分隔符，
// fmt.Scanf为输入数据指定了具体的输入内容格式，只有按照格式输入数据才会被扫描并存入对应变量。

```

### Scanln

```go
package main

import (
	"fmt"
)

func main() {
	var (
		name    string
		age     int
		married bool
	)
	fmt.Scanln(&name, &age, &married)
	fmt.Printf("扫描结果 name:%s age:%d married:%t \n", name, age, married)
}

// Scanln类似Scan，它在遇到换行时才停止扫描。最后一个数据后面必须有换行或者到达结束位置。
// 本函数返回成功扫描的数据个数和遇到的任何错误。

```

### bufio.NewReader

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

func main() {

	// 有时候我们想完整获取输入的内容，而输入的内容可能包含空格，
	// 这种情况下可以使用bufio包来实现。
	reader := bufio.NewReader(os.Stdin) // 从标准输入生成读对象
	fmt.Print("请输入内容：")
	text, _ := reader.ReadString('\n') // 读到换行
	text = strings.TrimSpace(text)
	fmt.Printf("%#v\n", text)
	fmt.Printf("%v\n", text)
}
```

