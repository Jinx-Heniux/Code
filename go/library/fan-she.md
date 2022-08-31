# Reflection 1

## [反射 · Go语言中文文档](https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/%E5%8F%8D%E5%B0%84.html)

### 空接口与反射

### 反射获取interface类型信息

```go
package main

import (
	"fmt"
	"reflect"
)

//反射获取interface类型信息

func reflect_type(a interface{}) {
	t := reflect.TypeOf(a)
	// fmt.Println("类型是：", t)
	fmt.Printf("t: %T, %v\n", t, t)
	// kind()可以获取具体类型
	k := t.Kind()
	// fmt.Println(k)
	fmt.Printf("t: %T, %v\n", t, t)
	switch k {
	case reflect.Float64:
		fmt.Printf("a is float64\n")
	case reflect.String:
		fmt.Println("string")
	}
}

func main() {
	var x float64 = 3.4
	reflect_type(x)
}

/*
t: *reflect.rtype, float64
t: *reflect.rtype, float64
a is float64
*/


```



### 反射获取interface值信息

```go
package main

import (
	"fmt"
	"reflect"
)

//反射获取interface值信息

func reflect_value(a interface{}) {
	v := reflect.ValueOf(a)
	fmt.Println(v)
	k := v.Kind()
	fmt.Println(k)
	switch k {
	case reflect.Float64:
		fmt.Println("a是：", v.Float())
	}
}

func main() {
	var x float64 = 3.4
	reflect_value(x)
}

/*
3.4
float64
a是： 3.4
*/


```



### 反射修改值信息

```go
package main

import (
	"fmt"
	"reflect"
)

//反射修改值
func reflect_set_value(a interface{}) {
	v := reflect.ValueOf(a)
	fmt.Printf("v: %T, %v\n", v, v)
	k := v.Kind()
	fmt.Printf("k: %T, %v\n", k, k)
	switch k {
	case reflect.Float64:
		// 反射修改值
		v.SetFloat(6.9)
		fmt.Println("a is ", v.Float())
	case reflect.Ptr:
		// Elem()获取地址指向的值
		v.Elem().SetFloat(7.9)
		fmt.Println("case:", v.Elem().Float())
		// 地址
		fmt.Println(v.Pointer())
	}
}

func main() {
	var x float64 = 3.4
	// 反射认为下面是指针类型，不是float类型
	reflect_set_value(&x)
	fmt.Println("main:", x)
}

/*
v: reflect.Value, 0xc0000ba000
k: reflect.Kind, ptr
case: 7.9
824634482688
main: 7.9
*/


```



### 反射修改值信息2

```go
package main

import (
	"fmt"
	"reflect"
)

//反射修改值
func reflect_set_value(a interface{}) {
	v := reflect.ValueOf(a)
	fmt.Printf("v: %T, %v\n", v, v)
	k := v.Kind()
	fmt.Printf("k: %T, %v\n", k, k)
	switch k {
	case reflect.Float64:
		// 反射修改值
		v.SetFloat(6.9)
		fmt.Println("a is ", v.Float())
	case reflect.Ptr:
		// Elem()获取地址指向的值
		v.Elem().SetFloat(7.9)
		fmt.Println("case:", v.Elem().Float())
		// 地址
		fmt.Println(v.Pointer())
	}
}

func main() {
	var x float64 = 3.4
	// 反射认为下面是指针类型，不是float类型
	reflect_set_value(x)
	fmt.Println("main:", x)
}

/*
v: reflect.Value, 3.4
k: reflect.Kind, float64
panic: reflect: reflect.Value.SetFloat using unaddressable value

goroutine 1 [running]:
reflect.flag.mustBeAssignableSlow(0x19?)
	/usr/local/go/src/reflect/value.go:262 +0x85
reflect.flag.mustBeAssignable(...)
	/usr/local/go/src/reflect/value.go:249
reflect.Value.SetFloat({0x486460?, 0xc00001c0f8?, 0x496936?}, 0x401b99999999999a)
	/usr/local/go/src/reflect/value.go:2147 +0x49
main.reflect_set_value({0x486460?, 0xc00001c0f8?})
	/home/zhs2si/go/src/hello/main.go:17 +0x397
main.main()
	/home/zhs2si/go/src/hello/main.go:31 +0x36
exit status 2
*/

```



### 结构体与反射

### 查看类型、字段和方法

```go
package main

import (
    "fmt"
    "reflect"
)

// 定义结构体
type User struct {
    Id   int
    Name string
    Age  int
}

// 绑方法
func (u User) Hello() {
    fmt.Println("Hello")
}

// 传入interface{}
func Poni(o interface{}) {
    t := reflect.TypeOf(o)
    fmt.Println("类型：", t)
    fmt.Println("字符串类型：", t.Name())
    // 获取值
    v := reflect.ValueOf(o)
    fmt.Println(v)
    // 可以获取所有属性
    // 获取结构体字段个数：t.NumField()
    for i := 0; i < t.NumField(); i++ {
        // 取每个字段
        f := t.Field(i)
        fmt.Printf("%s : %v", f.Name, f.Type)
        // 获取字段的值信息
        // Interface()：获取字段对应的值
        val := v.Field(i).Interface()
        fmt.Println("val :", val)
    }
    fmt.Println("=================方法====================")
    for i := 0; i < t.NumMethod(); i++ {
        m := t.Method(i)
        fmt.Println(m.Name)
        fmt.Println(m.Type)
    }

}

func main() {
    u := User{1, "zs", 20}
    Poni(u)
}

/*
类型： main.User
字符串类型： User
{1 zs 20}
Id : intval : 1
Name : stringval : zs
Age : intval : 20
=================方法====================
Hello
func(main.User)
*/

```



### 查看匿名字段

```go
package main

import (
    "fmt"
    "reflect"
)

// 定义结构体
type User struct {
    Id   int
    Name string
    Age  int
}

// 匿名字段
type Boy struct {
    User
    Addr string
}

func main() {
    m := Boy{User{1, "zs", 20}, "bj"}
    t := reflect.TypeOf(m)
    fmt.Println(t)
    // Anonymous：匿名
    fmt.Printf("%#v\n", t.Field(0))
    // 值信息
    fmt.Printf("%#v\n", reflect.ValueOf(m).Field(0))
}

/*
main.Boy
reflect.StructField{Name:"User", PkgPath:"", Type:(*reflect.rtype)(0x4b1d20), Tag:"", Offset:0x0, Index:[]int{0}, Anonymous:true}
main.User{Id:1, Name:"zs", Age:20}
*/

```



### 修改结构体的值

```go
package main

import (
	"fmt"
	"reflect"
)

// 定义结构体
type User struct {
	Id   int
	Name string
	Age  int
}

// 修改结构体值
func SetValue(o interface{}) {
	v := reflect.ValueOf(o)
	// 获取指针指向的元素
	v = v.Elem()
	// 取字段
	f := v.FieldByName("Name")
	if f.Kind() == reflect.String {
		f.SetString("kuteng")
	}
}

func main() {
	u := User{1, "5lmh.com", 20}
	SetValue(&u)
	fmt.Println(u)
}

/*
{1 kuteng 20}
*/


```



### 调用方法

```go
package main

import (
	"fmt"
	"reflect"
)

// 定义结构体
type User struct {
	Id   int
	Name string
	Age  int
}

func (u User) Hello(name string) {
	fmt.Println("Hello：", name)
}

func main() {
	u := User{1, "5lmh.com", 20}
	v := reflect.ValueOf(u)
	// 获取方法
	m := v.MethodByName("Hello")
	// 构建一些参数
	args := []reflect.Value{reflect.ValueOf("6666")}
	// 没参数的情况下：var args2 []reflect.Value
	// 调用方法，需要传入方法的参数
	m.Call(args)
}

/*
Hello： 6666
*/



```



### 获取字段的tag

```go
package main

import (
	"fmt"
	"reflect"
)

type Student struct {
	Name string `json:"name1" db:"name2"`
}

func main() {
	var s Student
	v := reflect.ValueOf(&s)
	// 类型
	t := v.Type()
	// 获取字段
	f := t.Elem().Field(0)
	fmt.Println(f.Tag.Get("json"))
	fmt.Println(f.Tag.Get("db"))
}

/*
name1
name2
*/


```



## [9、反射reflect · 语雀](https://www.yuque.com/aceld/mo95lb/cwur9v)

### 接口变量内部(值value；类型concrete type) pair

```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	tty, err := os.OpenFile("./names.txt", os.O_RDWR, 0)
	if err != nil {
		fmt.Println("open file error", err)
		return
	}
	fmt.Printf("tty: %T, %v,%p,%p\n", tty, tty, &tty, tty)

	var r io.Reader
	fmt.Printf("r: %T, %v,%p,%p\n", r, r, &r, r)
	r = tty
	fmt.Printf("r: %T, %v,%p,%p\n", r, r, &r, r)

	var w io.Writer
	fmt.Printf("w: %T, %v,%p,%p\n", w, w, &w, w)
	w = r.(io.Writer)
	fmt.Printf("w: %T, %v,%p,%p\n", w, w, &w, w)
	w.Write([]byte("HELLO THIS IS A TEST!!!\n"))
}

/*
tty: *os.File, &{0xc0000b6120},0xc0000b4018,0xc0000b4020
r: <nil>, <nil>,0xc00009e210,%!p(<nil>)
r: *os.File, &{0xc0000b6120},0xc00009e210,0xc0000b4020
w: <nil>, <nil>,0xc00009e220,%!p(<nil>)
w: *os.File, &{0xc0000b6120},0xc00009e220,0xc0000b4020
*/



```

### 接口变量内部(值value；类型concrete type) pair

```go
package main

import "fmt"

type Reader interface {
	ReadBook()
}

type Writer interface {
	WriteBook()
}

//具体类型
type Book struct {
}

func (this *Book) ReadBook() {
	fmt.Println("Read a book.")
}

func (this *Book) WriteBook() {
	fmt.Println("Write a book.")
}

func main() {
	b := &Book{}

	var r Reader
	r = b

	r.ReadBook()

	var w Writer
	w = r.(Writer)
	w.WriteBook()
}

/*
Read a book.
Write a book.
*/


```



### reflect的基本功能TypeOf和ValueOf

reflect.TypeOf()是获取pair中的type，reflect.ValueOf()获取pair中的value

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	var num float64 = 1.2345

	fmt.Println("type: ", reflect.TypeOf(num))
	fmt.Println("value: ", reflect.ValueOf(num))
}

/*
type:  float64
value:  1.2345
*/


```

