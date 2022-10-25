---
description: 切片
---

# Slice

## [Slice底层实现 · Go语言中文文档](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Slice%E5%BA%95%E5%B1%82%E5%AE%9E%E7%8E%B0.html)

### Go 数组是值类型

```go
package main

import (
	"fmt"
)

func main() {
	arrayA := [2]int{100, 200}
	var arrayB [2]int

	arrayB = arrayA

	fmt.Printf("arrayA : %p , %v\n", &arrayA, arrayA)
	fmt.Printf("arrayB : %p , %v\n", &arrayB, arrayB)

	testArray(arrayA)
}

func testArray(x [2]int) {
	fmt.Printf("func Array : %p , %v\n", &x, x)
}
// 在 Go 中，与 C 数组变量隐式作为指针使用不同，
// Go 数组是值类型，赋值和函数传参操作都会复制整个数组数据。
// arrayA : 0xc000138000 , [100 200]
// arrayB : 0xc000138010 , [100 200]
// func Array : 0xc000138050 , [100 200]
// 三个内存地址都不同，这也就验证了 Go 中数组赋值和函数传参都是值复制的。
```



### 函数传参用数组的指针

```go
package main

import (
	"fmt"
)

func main() {
	arrayA := [2]int{100, 200}
	fmt.Printf("(%T)(%d)(%d)(%p)\n", arrayA, len(arrayA), cap(arrayA), &arrayA)
	fmt.Printf("(%T)(%d)(%d)(%p)\n", arrayA, len(arrayA), cap(arrayA), &arrayA[0])
	testArrayAPoint(&arrayA) // 1.传数组指针
	arrayB := arrayA[:]
	testArrayBPoint(&arrayB) // 2.传切片
	fmt.Printf("arrayA : %p , %v\n", &arrayA, arrayA)
	fmt.Printf("arrayB : %p , %v\n", &arrayB, arrayB)
	fmt.Printf("arrayB[0] : %p , %v\n", &arrayB[0], arrayB[0])
}

func testArrayAPoint(x *[2]int) {
	fmt.Printf("func ArrayA : %p , %v\n", x, *x)
	(*x)[1] += 100
}

func testArrayBPoint(x *[]int) {
	fmt.Printf("func ArrayB : %p , %v\n", x, *x)
	(*x)[1] += 100
}

```



### 从 slice 中得到一块内存地址

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	s := make([]byte, 200)
	ptr := unsafe.Pointer(&s[0]) // 从 slice 中得到一块内存地址
	fmt.Printf("&s:%p | &arr:%p | %p\n", &s, ptr, s)
	// &s:0xc00000c030 | &arr:0xc00007e000 | 0xc00007e000
}
```



### unsafe.Sizeof((\*byte)(nil))

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	const ptrSize = unsafe.Sizeof((*byte)(nil))
	fmt.Println(ptrSize) // 8
}
```



### 扩容策略

```go
package main

import (
	"fmt"
)

func main() {
	slice := []int{10, 20, 30, 40}
	newSlice := append(slice, 50)
	fmt.Printf("Before slice = %v, Pointer = %p, len = %d, cap = %d\n", slice, &slice, len(slice), cap(slice))
	fmt.Printf("Before newSlice = %v, Pointer = %p, len = %d, cap = %d\n", newSlice, &newSlice, len(newSlice), cap(newSlice))
	newSlice[1] += 10
	fmt.Printf("After slice = %v, Pointer = %p, len = %d, cap = %d\n", slice, &slice, len(slice), cap(slice))
	fmt.Printf("After newSlice = %v, Pointer = %p, len = %d, cap = %d\n", newSlice, &newSlice, len(newSlice), cap(newSlice))
}

/*
Before slice = [10 20 30 40], Pointer = 0xc0000ac018, len = 4, cap = 4
Before newSlice = [10 20 30 40 50], Pointer = 0xc0000ac030, len = 5, cap = 8
After slice = [10 20 30 40], Pointer = 0xc0000ac018, len = 4, cap = 4
After newSlice = [10 30 30 40 50], Pointer = 0xc0000ac030, len = 5, cap = 8
*/

```



### 切片拷贝

```go
package main

import (
	"fmt"
)

func main() {
	array := []int{10, 20, 30, 40}
	slice := make([]int, 6)
	n := copy(slice, array)
	fmt.Println(n, slice) // 4 [10 20 30 40 0 0]
}

```



```go
package main

import (
	"fmt"
)

func main() {
	slice := make([]byte, 3)
	n := copy(slice, "abcdef")
	fmt.Println(n, slice) // 3 [97 98 99]
	fmt.Printf("%#v | %T | %v | %p | %p | %p | %v\n", slice, slice, slice, slice, &slice[0], &slice, string(slice))
	// []byte{0x61, 0x62, 0x63} | []uint8 | [97 98 99] | 0xc00012a000 | 0xc00012a000 | 0xc00011c018 | abc
}


```



```go
package main

import (
	"fmt"
)

func main() {
	slice := []int{10, 20, 30, 40}
	for index, value := range slice {
		fmt.Printf("index=%d value=%d value-addr=%x slice-addr=%x\n", index, value, &value, &slice[index])
	}
}

/*
index=0 value=10 value-addr=c0000140b8 slice-addr=c00001c120
index=1 value=20 value-addr=c0000140b8 slice-addr=c00001c128
index=2 value=30 value-addr=c0000140b8 slice-addr=c00001c130
index=3 value=40 value-addr=c0000140b8 slice-addr=c00001c138
*/

```



## [6、slice和map · 语雀](https://www.yuque.com/aceld/mo95lb/ovmzgh)



```go
package main

import "fmt"

func main() {
	var numbers = make([]int, 3, 5)
	numbers[2] = 100
	fmt.Printf("address of number: %p | numbers: %v\n", &numbers, numbers)
	printSlice(numbers)
}

func printSlice(x []int) {
	x[2] = 200
	fmt.Printf("address of x: %p | x: %v\n", &x, x)
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

/*
address of number: 0xc0000ac018 | numbers: [0 0 100]
address of x: 0xc0000ac048 | x: [0 0 200]
len=3 cap=5 slice=[0 0 200]
*/


```



### 空切片 nil

```go
package main

import "fmt"

func main() {
	var numbers []int
	fmt.Printf("address of number: %p | numbers: %v\n", &numbers, numbers)
	printSlice(numbers)

	if numbers == nil {
		fmt.Println("slice is nil")
	}
}

func printSlice(x []int) {
	fmt.Printf("address of x: %p | x: %v\n", &x, x)
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

/*
address of number: 0xc00000c030 | numbers: []
address of x: 0xc00000c048 | x: []
len=0 cap=0 slice=[]
slice is nil
*/


```



### 切片截取

```go
package main

import "fmt"

func main() {
	/* 创建切片 */
	numbers := []int{0, 1, 2, 3, 4, 5, 6, 7, 8}
	printSlice(numbers)

	/* 打印原始切片 */
	fmt.Println("numbers ==", numbers)

	/* 打印子切片从索引1(包含) 到索引4(不包含)*/
	fmt.Println("numbers[1:4] ==", numbers[1:4])

	/* 默认下限为 0*/
	fmt.Println("numbers[:3] ==", numbers[:3])

	/* 默认上限为 len(s)*/
	fmt.Println("numbers[4:] ==", numbers[4:])

	numbers1 := make([]int, 0, 5)
	printSlice(numbers1)

	/* 打印子切片从索引  0(包含) 到索引 2(不包含) */
	number2 := numbers[:2]
	printSlice(number2)

	/* 打印子切片从索引 2(包含) 到索引 5(不包含) */
	number3 := numbers[2:5]
	printSlice(number3)

}

func printSlice(x []int) {
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

/*
len=9 cap=9 slice=[0 1 2 3 4 5 6 7 8]
numbers == [0 1 2 3 4 5 6 7 8]
numbers[1:4] == [1 2 3]
numbers[:3] == [0 1 2]
numbers[4:] == [4 5 6 7 8]
len=0 cap=5 slice=[]
len=2 cap=9 slice=[0 1]
len=3 cap=7 slice=[2 3 4]
*/


```



### append() 和 copy() 函数

```go
package main

import "fmt"

func main() {
	var numbers []int
	printSlice(numbers)

	/* 允许追加空切片 */
	numbers = append(numbers, 0)
	printSlice(numbers)

	/* 向切片添加一个元素 */
	numbers = append(numbers, 1)
	printSlice(numbers)

	/* 同时添加多个元素 */
	numbers = append(numbers, 2, 3, 4)
	printSlice(numbers)

	/* 创建切片 numbers1 是之前切片的两倍容量*/
	numbers1 := make([]int, len(numbers), (cap(numbers))*2)

	/* 拷贝 numbers 的内容到 numbers1 */
	copy(numbers1, numbers)
	printSlice(numbers1)
}

func printSlice(x []int) {
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

/*
len=0 cap=0 slice=[]
len=1 cap=1 slice=[0]
len=2 cap=2 slice=[0 1]
len=5 cap=6 slice=[0 1 2 3 4]
len=5 cap=12 slice=[0 1 2 3 4]
*/


```

