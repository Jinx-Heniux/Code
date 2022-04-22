# fmt

## fmt · Go语言中文文档

[https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/fmt.html](https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/fmt.html)

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
```

