# 基础

## 变量和常量 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%8F%98%E9%87%8F%E5%92%8C%E5%B8%B8%E9%87%8F.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%8F%98%E9%87%8F%E5%92%8C%E5%B8%B8%E9%87%8F.html)

### 短变量声明

```go
package main

import (
	"fmt"
)

// 全局变量m
var m = 100

func main() {
	n := 10 // 在函数内部，可以使用更简略的 := 方式声明并初始化变量。
	// m := 200 // 此处声明局部变量m
	fmt.Println(m, n)
}

```



### 匿名变量

```go
package main

import (
	"fmt"
)

func foo() (int, string) {
	return 10, "Q1mi"
}

func main() {
	x, _ := foo() // 匿名变量用一个下划线_表示
	_, y := foo() // 匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。
	fmt.Println("x=", x)
	fmt.Println("y=", y)
}

```



## 基本类型 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B.html)



```go
package main

import "fmt"

func main() {

	fmt.Println("str := \"c:\\pprof\\main.exe\"")
	// str := "c:\pprof\main.exe"
	// 反斜杠

	// 反引号字符 多行字符串
	s1 := `第一行
    第二行
    第三行
    `
	fmt.Println(s1)

	/*
		组成每个字符串的元素叫做“字符”，可以通过遍历或者单个获取字符串元素获得字符。
		字符用单引号（’）包裹起来.

		uint8类型，或者叫 byte 型，代表了ASCII码的一个字符。
		rune类型，代表一个 UTF-8字符。
	*/
	z_ch := '中'
	e_ch := 'A'
	fmt.Printf("z_ch -> %c (%#v) (%T)\n", z_ch, z_ch, z_ch)
	fmt.Printf("e_ch -> %c (%#v) (%T)\n", e_ch, e_ch, e_ch)

	// z_ch -> 中 (20013) (int32)
	// e_ch -> A (65) (int32)
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
	//for _, r := range s { //rune
	for i, r := range s {
		// rune类型用来表示utf8字符，一个rune字符由一个或多个byte组成。
		fmt.Printf("%d -> %v(%c)(%T) \n", i, r, r, r)
		rune_nums += 1
	}
	fmt.Println()
	fmt.Printf("numbers of runes: %d\n", rune_nums)
}

/*
s -> pprof.cn博客(pprof.cn博客)(string)(14)

112(p)(uint8) 112(p)(uint8) 114(r)(uint8) 111(o)(uint8) 102(f)(uint8) 46(.)(uint8) 99(c)(uint8) 110(n)(uint8) 229(å)(uint8) 141()(uint8) 154()(uint8) 229(å)(uint8) 174(®)(uint8) 162(¢)(uint8)

0 -> 112(p)(int32)
1 -> 112(p)(int32)
2 -> 114(r)(int32)
3 -> 111(o)(int32)
4 -> 102(f)(int32)
5 -> 46(.)(int32)
6 -> 99(c)(int32)
7 -> 110(n)(int32)
8 -> 21338(博)(int32)
11 -> 23458(客)(int32)

numbers of runes: 10
*/



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
	fmt.Printf("byteS1: %v (%c) (%#v) (%T) (%d)\n", byteS1, byteS1, byteS1, byteS1, len(byteS1))
	fmt.Println()

	byteS1[0] = 'H'
	fmt.Println(string(byteS1))

	s2 := "博客"
	runeS2 := []rune(s2)
	fmt.Printf("runeS2: %v (%c) (%#v) (%T) (%d)\n", runeS2, runeS2, runeS2, runeS2, len(runeS2))
	fmt.Println()

	runeS2[0] = '狗'
	fmt.Println(string(runeS2))
}

/*
byteS1: [104 101 108 108 111] ([h e l l o]) ([]byte{0x68, 0x65, 0x6c, 0x6c, 0x6f}) ([]uint8) (5)

Hello
runeS2: [21338 23458] ([博 客]) ([]int32{21338, 23458}) ([]int32) (2)

狗客
*/


```

