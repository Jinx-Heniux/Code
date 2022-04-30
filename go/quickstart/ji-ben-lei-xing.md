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
	fmt.Printf("s -> %v(%s)(%T)(%d)\n", s, s, s, len(s))
	fmt.Println()
	for i := 0; i < len(s); i++ { //byte
	// 字符串底层是一个byte数组，所以可以和[]byte类型相互转换。
		fmt.Printf("%v(%c)(%T) ", s[i], s[i], s[i])
	}
	fmt.Println()
	fmt.Println()
	rune_nums := 0
	for _, r := range s { //rune
	// rune类型用来表示utf8字符，一个rune字符由一个或多个byte组成。
		fmt.Printf("%v(%c)(%T) ", r, r, r)
		rune_nums += 1
	}
	fmt.Println()
	fmt.Printf("numbers of runes: %d\n", rune_nums)
}

// s -> pprof.cn博客(pprof.cn博客)(string)(14)
// 112(p)(uint8) 112(p)(uint8) 114(r)(uint8) 111(o)(uint8) 102(f)(uint8) 46(.)(uint8) 99(c)(uint8) 110(n)(uint8) 229(å)(uint8) 141()(uint8) 154()(uint8) 229(å)(uint8) 174(®)(uint8) 162(¢)(uint8)
// 112(p)(int32) 112(p)(int32) 114(r)(int32) 111(o)(int32) 102(f)(int32) 46(.)(int32) 99(c)(int32) 110(n)(int32) 21338(博)(int32) 23458(客)(int32)
// numbers of runes: 10


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
	fmt.Printf("byteS1: %v(%T)(%d)", byteS1, byteS1, len(byteS1))
	fmt.Println()

	byteS1[0] = 'H'
	fmt.Println(string(byteS1))

	s2 := "博客"
	runeS2 := []rune(s2)
	fmt.Printf("runeS2: %v(%T)(%d)", runeS2, runeS2, len(runeS2))
	fmt.Println()

	runeS2[0] = '狗'
	fmt.Println(string(runeS2))
}

```

