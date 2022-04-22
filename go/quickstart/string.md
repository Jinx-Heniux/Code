# String

## 基本类型 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B.html)

### 遍历字符串

```go
package main

import (
	"fmt"
)

func main() {
	// 遍历字符串 traversalString()
	s := "pprof.cn博客"
	fmt.Printf("%v(%s)\n", s, s)
	fmt.Println()
	for i := 0; i < len(s); i++ { //byte
		fmt.Printf("%v(%c) ", s[i], s[i])
	}
	fmt.Println()
	for _, r := range s { //rune
		fmt.Printf("%v(%c) ", r, r)
	}
	fmt.Println()
}
```

