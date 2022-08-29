---
description: >-
  https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/%E5%8F%8D%E5%B0%84.html
---

# 反射 · Go语言中文文档

link

## 空接口与反射

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



## 结构体与反射

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

