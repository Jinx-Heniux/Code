# 基本类型

## 基本类型 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B.html)



```go
package main

import "fmt"

func main() {

	z_ch := '中'
	e_ch := 'A'
	fmt.Printf("z_ch -> %v(%T)\n", z_ch, z_ch)
	fmt.Printf("e_ch -> %v(%T)\n", e_ch, e_ch)

	// z_ch -> 20013(int32)
	// e_ch -> 65(int32)
	// 当需要处理中文、日文或者其他复合字符时，则需要用到rune类型。
	// rune类型实际是一个int32。
}

```



### 遍历字符串

```go
package main

import (
	"fmt"
)

func main() {
	// 遍历字符串 traversalString()
	s := "pprof.cn博客"
	fmt.Printf("s -> %v(%T)\n", s, s)
	fmt.Println()
	for i := 0; i < len(s); i++ { //byte
		fmt.Printf("%v(%c)(%T) ", s[i], s[i], s[i])
	}
	fmt.Println()
	fmt.Println()
	for _, r := range s { //rune
		fmt.Printf("%v(%c)(%T) ", r, r, r)
	}
	fmt.Println()
}

```

### 修改字符串

```go
package main

import (
	"fmt"
)

func main() {
	s1 := "hello"
	// 强制类型转换
	byteS1 := []byte(s1)
	fmt.Printf("byteS1: %v(%T)", byteS1, byteS1)
	fmt.Println()

	byteS1[0] = 'H'
	fmt.Println(string(byteS1))

	s2 := "博客"
	runeS2 := []rune(s2)
	fmt.Printf("runeS2: %v(%T)", runeS2, runeS2)
	fmt.Println()

	runeS2[0] = '狗'
	fmt.Println(string(runeS2))
}
```

