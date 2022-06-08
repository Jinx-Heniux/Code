# 方法集

## [方法集 · Go语言中文文档](https://www.topgoer.com/%E6%96%B9%E6%B3%95/%E6%96%B9%E6%B3%95%E9%9B%86.html)

### 类型 T 方法集包含全部 receiver T 方法

```go
package main

import (
	"fmt"
)

type T struct {
	int
}

func (t T) test() {
	fmt.Println("类型 T 方法集包含全部 receiver T 方法。")
}

func main() {
	t1 := T{1}
	fmt.Printf("t1 is : %v\n", t1)
	t1.test()
}

/*
t1 is : {1}
类型 T 方法集包含全部 receiver T 方法。
*/

```
