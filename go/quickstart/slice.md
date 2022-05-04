# Slice

## 切片Slice · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%88%87%E7%89%87Slice.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%88%87%E7%89%87Slice.html)

### 创建切片

```go
package main

import (
	"fmt"
)

func main() {

	var str string
	fmt.Printf("str -> %v(%T)\n", str, str)
	if str == "" {
		fmt.Println("是空")
	} else {
		fmt.Println("不是空")
	}

	//1.声明切片
	var s1 []int
	fmt.Printf("s1 -> %v(%T)\n", s1, s1)
	if s1 == nil {
		fmt.Println("是空")
	} else {
		fmt.Println("不是空")
	}

	// 2.:=
	s2 := []int{}
	fmt.Printf("s2 -> %v(%T)\n", s2, s2)

	// 3.make()
	var s3 []int = make([]int, 0)
	fmt.Printf("s3 -> %v(%T)\n", s3, s3)

	// 4.初始化赋值
	var s4 []int = make([]int, 0, 0)
	fmt.Printf("s4 -> %v(%T)\n", s4, s4)

	s5 := []int{1, 2, 3}
	fmt.Printf("s5 -> %v(%T)(%d)(%d)\n", s5, s5, len(s5), cap(s5))

	// 5.从数组切片
	arr := [5]int{1, 2, 3, 4, 5}
	fmt.Printf("arr -> %v(%T)(%d)(%d)\n", arr, arr, len(arr), cap(arr))
	var s6 []int
	// 前包后不包
	s6 = arr[1:4]
	fmt.Printf("s6 -> %v(%T)(%d)(%d)\n", s6, s6, len(s6), cap(s6))
}

```

### 切片初始化

```go
package main

import (
	"fmt"
)

var arr = [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
var slice0 []int = arr[2:8]
var slice1 []int = arr[0:6]        //可以简写为 var slice []int = arr[:end]
var slice2 []int = arr[5:10]       //可以简写为 var slice[]int = arr[start:]
var slice3 []int = arr[0:len(arr)] //var slice []int = arr[:]
var slice4 = arr[:len(arr)-1]      //去掉切片的最后一个元素

func main() {
	fmt.Printf("(%T)(%d)(%d)", arr, len(arr), cap(arr))
	fmt.Printf("全局变量: arr %v\n", arr)

	fmt.Printf("(%T)(%d)(%d)", slice0, len(slice0), cap(slice0))
	fmt.Printf("全局变量: slice0 %v\n", slice0)

	fmt.Printf("(%T)(%d)(%d)", slice1, len(slice1), cap(slice1))
	fmt.Printf("全局变量: slice1 %v\n", slice1)
	fmt.Printf("全局变量: slice2 %v\n", slice2)
	fmt.Printf("全局变量: slice3 %v\n", slice3)
	fmt.Printf("全局变量: slice4 %v\n", slice4)
	fmt.Printf("-----------------------------------\n")

	arr2 := [...]int{9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
	slice5 := arr[2:8]
	slice6 := arr[0:6]         //可以简写为 slice := arr[:end]
	slice7 := arr[5:10]        //可以简写为 slice := arr[start:]
	slice8 := arr[0:len(arr)]  //slice := arr[:]
	slice9 := arr[:len(arr)-1] //去掉切片的最后一个元素
	fmt.Printf("(%T)(%d)(%d)", arr2, len(arr2), cap(arr2))
	fmt.Printf("局部变量:  arr2 %v\n", arr2)
	fmt.Printf("局部变量:  slice5 %v\n", slice5)
	fmt.Printf("局部变量:  slice6 %v\n", slice6)
	fmt.Printf("局部变量:  slice7 %v\n", slice7)
	fmt.Printf("局部变量:  slice8 %v\n", slice8)
	fmt.Printf("局部变量:  slice9 %v\n", slice9)
}

```

### 通过make来创建切片

```go
package main

import (
	"fmt"
)

var slice0 []int = make([]int, 10)
var slice1 = make([]int, 10)
var slice2 = make([]int, 10, 10)

func main() {
	fmt.Printf("(%T)(%d)(%d)", slice0, len(slice0), cap(slice0))
	fmt.Printf("make全局slice0 : %v\n", slice0)
	fmt.Printf("(%T)(%d)(%d)", slice1, len(slice1), cap(slice1))
	fmt.Printf("make全局slice1 : %v\n", slice1)
	fmt.Printf("(%T)(%d)(%d)", slice2, len(slice2), cap(slice2))
	fmt.Printf("make全局slice2 : %v\n", slice2)
	fmt.Println("--------------------------------------")
	slice3 := make([]int, 10)
	slice4 := make([]int, 10)
	slice5 := make([]int, 10, 10)
	fmt.Printf("(%T)(%d)(%d)", slice3, len(slice3), cap(slice3))
	fmt.Printf("make局部slice3 : %v\n", slice3)
	fmt.Printf("make局部slice4 : %v\n", slice4)
	fmt.Printf("make局部slice5 : %v\n", slice5)
}

```

### 读写操作实际目标是底层数组

```go
package main

import (
	"fmt"
)

func main() {
	data := [...]int{0, 1, 2, 3, 4, 5}

	s := data[2:4]
	s[0] += 100
	s[1] += 200

	fmt.Println(s)
	fmt.Println(data)
}
```

### 直接创建 slice 对象

```go
package main

import (
	"fmt"
)

func main() {
	s1 := []int{0, 1, 2, 3, 8: 100} // 通过初始化表达式构造，可使用索引号。
	fmt.Println(s1, len(s1), cap(s1))

	s2 := make([]int, 6, 8) // 使用 make 创建，指定 len 和 cap 值。
	fmt.Println(s2, len(s2), cap(s2))

	s3 := make([]int, 6) // 省略 cap，相当于 cap = len。
	fmt.Println(s3, len(s3), cap(s3))
}

```



```go
package main

import (
	"fmt"
)

func main() {
	s := []int{0, 1, 2, 3}
	p := &s[2] // *int, 获取底层数组元素指针。
	*p += 100

	fmt.Println(s) // [0 1 102 3]
}

```

### \[]\[]T 指切片中的元素类型为 \[]T

```go
package main

import (
	"fmt"
)

func main() {
	data := [][]int{
		[]int{1, 2, 3},
		[]int{100, 200},
		[]int{11, 22, 33, 44},
	}
	fmt.Printf("(%T)(%d)(%d)", data, len(data), cap(data))
	fmt.Println(data)
}


```

### 直接修改 struct array/slice 成员

```go
package main

import (
	"fmt"
)

func main() {
	d := [5]struct {
		x int
	}{}
	fmt.Printf("(%T)(%d)(%d)", d, len(d), cap(d))
	fmt.Println(d)

	s := d[:]
	fmt.Printf("(%T)(%d)(%d)", s, len(s), cap(s))
	fmt.Println(s)

	d[1].x = 10
	s[2].x = 20

	fmt.Println(d)
	fmt.Printf("%p, %p\n", &d, &d[0])
	fmt.Printf("%p, %p, %p\n", &s, &s[0], s)

}


```

### 用append内置函数操作切片（切片追加）

append ：向 slice 尾部添加数据，返回新的 slice 对象。

```go
package main

import (
	"fmt"
)

func main() {

	var a = []int{1, 2, 3}
	fmt.Printf("(%T)(%d)(%d)(%p)", a, len(a), cap(a), &a[0])
	fmt.Printf("slice a : %v\n", a)

	var b = []int{4, 5, 6}
	fmt.Printf("(%T)(%d)(%d)(%p)", b, len(b), cap(b), &b[0])
	fmt.Printf("slice b : %v\n", b)

	c := append(a, b...)
	fmt.Printf("(%T)(%d)(%d)(%p)", c, len(c), cap(c), &c[0])
	fmt.Printf("slice c : %v\n", c)

	d := append(c, 7)
	fmt.Printf("(%T)(%d)(%d)(%p)", d, len(d), cap(d), &d[0])
	fmt.Printf("slice d : %v\n", d)

	e := append(d, 8, 9, 10)
	fmt.Printf("(%T)(%d)(%d)(%p)", e, len(e), cap(e), &e[0])
	fmt.Printf("slice e : %v\n", e)

	// ([]int)(3)(3)(0xc0000142d0)slice a : [1 2 3]
	// ([]int)(3)(3)(0xc0000142e8)slice b : [4 5 6]
	// ([]int)(6)(6)(0xc00001e360)slice c : [1 2 3 4 5 6]
	// ([]int)(7)(12)(0xc0000221e0)slice d : [1 2 3 4 5 6 7]
	// 底层数组翻倍增加，从6变成12
	// ([]int)(10)(12)(0xc0000221e0)slice e : [1 2 3 4 5 6 7 8 9 10]

}


```



```go
package main

import (
	"fmt"
)

func main() {

	s1 := make([]int, 0, 5)
	fmt.Printf("(%T)(%d)(%d)", s1, len(s1), cap(s1))
	fmt.Printf("address of s1->%p\n", &s1)

	s2 := append(s1, 1)
	fmt.Printf("(%T)(%d)(%d)(%p)", s2, len(s2), cap(s2), &s2[0])
	fmt.Printf("address of s2 -> %p\n", &s2)

	s3 := append(s1, 1, 2)
	fmt.Printf("(%T)(%d)(%d)(%p)", s3, len(s3), cap(s3), &s3[0])
	fmt.Printf("address of s3 -> %p\n", &s3)

	fmt.Println(s1, s2, s3)

	// ([]int)(0)(5)address of s1->0xc00000c030
	// ([]int)(1)(5)(0xc00001e360)address of s2 -> 0xc00000c060
	// ([]int)(2)(5)(0xc00001e360)address of s3 -> 0xc00000c090
	// [] [1] [1 2]
	// 底层的数组是同一个，地址为0xc00001e360

}


```

### 超出原 slice.cap 限制

```go
package main

import (
	"fmt"
)

func main() {

	data := [...]int{0, 1, 2, 3, 4, 10: 0}
	s := data[:2:3]
	fmt.Printf("(%T)(%d)(%d)(%p)", data, len(data), cap(data), &data[0])
	fmt.Printf("data -> %v\n", data)
	fmt.Printf("(%T)(%d)(%d)(%p)", s, len(s), cap(s), &s[0])
	fmt.Printf("s -> %v\n", s)
	fmt.Println()

	s = append(s, 100) // 一次 append 两个值，超出 s.cap 限制。
	fmt.Printf("(%T)(%d)(%d)(%p)", s, len(s), cap(s), &s[0])
	fmt.Printf("s -> %v\n", s)
	fmt.Printf("(%T)(%d)(%d)(%p)", data, len(data), cap(data), &data[0])
	fmt.Printf("data -> %v\n", data)
	fmt.Println()

	s = append(s, 100, 200) // 一次 append 两个值，超出 s.cap 限制。
	fmt.Printf("(%T)(%d)(%d)(%p)", s, len(s), cap(s), &s[0])
	fmt.Printf("s -> %v\n", s)
	fmt.Printf("(%T)(%d)(%d)(%p)", data, len(data), cap(data), &data[0])
	fmt.Printf("data -> %v\n", data)
	fmt.Println(s, data)         // 重新分配底层数组，与原数组无关。
	fmt.Println(&s[0], &data[0]) // 比对底层数组起始指针。
	fmt.Println(cap(s), cap(data))

}
// https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E5%88%87%E7%89%87Slice.html#%E8%B6%85%E5%87%BA%E5%8E%9F-slicecap-%E9%99%90%E5%88%B6%EF%BC%8C%E5%B0%B1%E4%BC%9A%E9%87%8D%E6%96%B0%E5%88%86%E9%85%8D%E5%BA%95%E5%B1%82%E6%95%B0%E7%BB%84%EF%BC%8C%E5%8D%B3%E4%BE%BF%E5%8E%9F%E6%95%B0%E7%BB%84%E5%B9%B6%E6%9C%AA%E5%A1%AB%E6%BB%A1%E3%80%82

```

### slice中cap重新分配规律

```go
package main

import (
	"fmt"
)

func main() {

	s := make([]int, 0, 1)
	c := cap(s)
	fmt.Printf("s:%p\n", &s)

	for i := 0; i < 50; i++ {
		s = append(s, i)
		fmt.Printf("i=%d || len= %d cap=%d || s: %p || s=%v\n", i, len(s), cap(s), &s, s)
		if n := cap(s); n > c {
			fmt.Printf("cap: %d -> %d\n", c, n)
			c = n
		}
	}

}

```



### 切片拷贝

```go
package main

import (
	"fmt"
)

func main() {

	s1 := []int{1, 2, 3, 4, 5}
	fmt.Printf("(%T)(%d)(%d)(%p)", s1, len(s1), cap(s1), &s1[0])
	fmt.Printf("slice s1 : %v\n", s1)

	s2 := make([]int, 10)
	fmt.Printf("(%T)(%d)(%d)(%p)", s2, len(s2), cap(s2), &s2[0])
	fmt.Printf("slice s2 : %v\n", s2)

	// copy(s1, s2)
	copy(s2, s1)
	fmt.Printf("(%T)(%d)(%d)(%p)", s1, len(s1), cap(s1), &s1[0])
	fmt.Printf("copied slice s1 : %v\n", s1)
	fmt.Printf("(%T)(%d)(%d)(%p)", s2, len(s2), cap(s2), &s2[0])
	fmt.Printf("copied slice s2 : %v\n", s2)

	s11 := []int{5, 4}
	fmt.Printf("(%T)(%d)(%d)(%p)", s11, len(s11), cap(s11), &s11[0])
	fmt.Printf("slice s11 : %v\n", s11)

	copy(s2, s11)
	fmt.Printf("(%T)(%d)(%d)(%p)", s11, len(s11), cap(s11), &s11[0])
	fmt.Printf("slice s11 : %v\n", s11)
	fmt.Printf("(%T)(%d)(%d)(%p)", s2, len(s2), cap(s2), &s2[0])
	fmt.Printf("copied slice s2 : %v\n", s2)

	s3 := []int{1, 2, 3}
	fmt.Printf("(%T)(%d)(%d)(%p)", s3, len(s3), cap(s3), &s3[0])
	fmt.Printf("slice s3 : %v\n", s3)

	s3 = append(s3, s2...)
	fmt.Printf("(%T)(%d)(%d)(%p)", s3, len(s3), cap(s3), &s3[0])
	fmt.Printf("appended slice s3 : %v\n", s3)

	s3 = append(s3, 4, 5, 6)
	fmt.Printf("(%T)(%d)(%d)(%p)", s3, len(s3), cap(s3), &s3[0])
	fmt.Printf("last slice s3 : %v\n", s3)

	// ([]int)(5)(5)(0xc000128030)slice s1 : [1 2 3 4 5]
	// ([]int)(10)(10)(0xc000136000)slice s2 : [0 0 0 0 0 0 0 0 0 0]
	// ([]int)(5)(5)(0xc000128030)copied slice s1 : [1 2 3 4 5]
	// ([]int)(10)(10)(0xc000136000)copied slice s2 : [1 2 3 4 5 0 0 0 0 0]
	// ([]int)(2)(2)(0xc000132110)slice s11 : [5 4]
	// ([]int)(2)(2)(0xc000132110)slice s11 : [5 4]
	// ([]int)(10)(10)(0xc000136000)copied slice s2 : [5 4 3 4 5 0 0 0 0 0]
	// ([]int)(3)(3)(0xc00013a000)slice s3 : [1 2 3]
	// ([]int)(13)(14)(0xc00013c000)appended slice s3 : [1 2 3 5 4 3 4 5 0 0 0 0 0]
	// 没明白什么是14？([]int)(13)(14)
	// ([]int)(16)(28)(0xc00013e000)last slice s3 : [1 2 3 5 4 3 4 5 0 0 0 0 0 4 5 6]
}

```



```go
package main

import (
	"fmt"
)

func main() {

	data := [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	fmt.Printf("(%T)(%d)(%d)(%p)", data, len(data), cap(data), &data[0])
	fmt.Println("array data : ", data)
	s1 := data[8:]
	s2 := data[:5]
	fmt.Printf("(%T)(%d)(%d)(%p)", s1, len(s1), cap(s1), &s1[0])
	fmt.Printf("slice s1 : %v\n", s1)
	fmt.Printf("(%T)(%d)(%d)(%p)", s2, len(s2), cap(s2), &s2[0])
	fmt.Printf("slice s2 : %v\n", s2)
	copy(s2, s1)
	fmt.Printf("(%T)(%d)(%d)(%p)", s1, len(s1), cap(s1), &s1[0])
	fmt.Printf("copied slice s1 : %v\n", s1)
	fmt.Printf("(%T)(%d)(%d)(%p)", s2, len(s2), cap(s2), &s2[0])
	fmt.Printf("copied slice s2 : %v\n", s2)
	fmt.Printf("(%T)(%d)(%d)(%p)", data, len(data), cap(data), &data[0])
	fmt.Println("last array data : ", data)

	// ([10]int)(10)(10)(0xc0000180a0)array data :  [0 1 2 3 4 5 6 7 8 9]
	// ([]int)(2)(2)(0xc0000180e0)slice s1 : [8 9]
	// ([]int)(5)(10)(0xc0000180a0)slice s2 : [0 1 2 3 4]
	// ([]int)(2)(2)(0xc0000180e0)copied slice s1 : [8 9]
	// ([]int)(5)(10)(0xc0000180a0)copied slice s2 : [8 9 2 3 4]
	// ([10]int)(10)(10)(0xc0000180a0)last array data :  [8 9 2 3 4 5 6 7 8 9]

}

```



### slice遍历

```go
package main

import (
	"fmt"
)

func main() {

	data := [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	fmt.Printf("(%T)(%d)(%d)(%p)\n", data, len(data), cap(data), &data[0])
	slice := data[:]
	fmt.Printf("(%T)(%d)(%d)(%p)\n", slice, len(slice), cap(slice), &slice[0])
	for index, value := range slice {
		fmt.Printf("inde : %v , value : %v\n", index, value)
	}

}

```



### 切片resize（调整大小）

```go
package main

import (
	"fmt"
)

func main() {
	var a = []int{1, 3, 4, 5}
	fmt.Printf("(%T)(%d)(%d)(%p)", a, len(a), cap(a), &a[0])
	fmt.Printf("slice a : %v , len(a) : %v\n", a, len(a))
	b := a[1:2]
	fmt.Printf("(%T)(%d)(%d)(%p)", b, len(b), cap(b), &b[0])
	fmt.Printf("slice b : %v , len(b) : %v\n", b, len(b))
	c := b[0:3]
	fmt.Printf("(%T)(%d)(%d)(%p)", c, len(c), cap(c), &c[0])
	fmt.Printf("slice c : %v , len(c) : %v\n", c, len(c))

	// ([]int)(4)(4)(0xc000016200)slice a : [1 3 4 5] , len(a) : 4
	// ([]int)(1)(3)(0xc000016208)slice b : [3] , len(b) : 1
	// ([]int)(3)(3)(0xc000016208)slice c : [3 4 5] , len(c) : 3
	// 注意c是从b做的切片

}

```



### 字符串和切片（string and slice）

string底层就是一个byte的数组，因此，也可以进行切片操作。

```go
package main

import (
	"fmt"
)

func main() {
	str := "hello world"
	fmt.Printf("(%T)(%d) str-> %s\n", str, len(str), str)
	s1 := str[0:5]
	fmt.Printf("(%T)(%d) ", s1, len(s1))
	fmt.Println(s1)

	s2 := str[6:]
	fmt.Printf("(%T)(%d) ", s2, len(s2))
	fmt.Println(s2)

	// (string)(11) str-> hello world
	// (string)(5) hello
	// (string)(5) world

}

```

string本身是不可变的，因此要改变string中字符。需要如下操作： 英文字符串：

```go
package main

import (
	"fmt"
)

func main() {
	str := "Hello world"
	s := []byte(str) //中文字符需要用[]rune(str)
	s[6] = 'G'
	s = s[:8]
	s = append(s, '!')
	str = string(s)
	fmt.Println(str)
}
```

含有中文字符串

```go
package main

import (
	"fmt"
)

func main() {
	str := "你好，世界！hello world！"
	s := []rune(str)
	s[3] = '够'
	s[4] = '浪'
	s[12] = 'g'
	s = s[:14]
	str = string(s)
	fmt.Println(str)
}

```

切片容量

```go
package main

import (
	"fmt"
)

func main() {
	slice := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	d1 := slice[6:8]
	fmt.Println(d1, len(d1), cap(d1))
	d2 := slice[:6:8]
	fmt.Println(d2, len(d2), cap(d2))
	d3 := slice[:6]
	fmt.Println(d3, len(d3), cap(d3))
}

```



## Slice底层实现 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Slice%E5%BA%95%E5%B1%82%E5%AE%9E%E7%8E%B0.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Slice%E5%BA%95%E5%B1%82%E5%AE%9E%E7%8E%B0.html)&#x20;

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

