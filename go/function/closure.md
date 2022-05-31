---
description: 闭包
---

# Closure

## [闭包、递归 · Go语言中文文档](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E9%97%AD%E5%8C%85%E9%80%92%E5%BD%92.html)



```go
package main

import (
	"fmt"
)

func a() func() int {
	i := 0
	b := func() int {
		i++
		fmt.Println(i)
		return i
	}
	return b
}

func main() {
	c := a()
	c()
	c()
	c()

	a() //不会输出i

	c2 := a()
	c2()
	c2()
	c2()
}

```

