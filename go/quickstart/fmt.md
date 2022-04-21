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



