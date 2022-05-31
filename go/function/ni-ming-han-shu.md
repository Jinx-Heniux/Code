# 匿名函数

## [匿名函数 · Go语言中文文档](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0.html)

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	getSqrt := func(a float64) float64 {
		return math.Sqrt(a)
	}
	fmt.Println(getSqrt(4))
}

```

