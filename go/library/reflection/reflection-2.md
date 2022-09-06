# Reflection 2

## [Go reflect 反射- Type & Value & Field & Method - 掘金](https://juejin.cn/post/6844904199994474504)

### Type

```go
package main

import (
	"fmt"
	"reflect"
)

type UserService interface {
	Service(str string) string
}

type userService struct {
	ServerName string
	Info       map[string]interface{}
	List       [10]int
}

func (*userService) Service(str string) string {
	return "name"
}

func main() {
	var in = (*UserService)(nil) //1、接口
	fmt.Printf("in: (%T) %v\n\n", in, in)

	inter := reflect.TypeOf(in)
	fmt.Printf("inter: (%T) %v\n", inter, inter)

	// inter_kind := inter.Kind()
	// fmt.Printf("inter_kind: (%T) %v\n", inter_kind, inter_kind)
	fmt.Printf("inter.Kind(): (%T) %v\n", inter.Kind(), inter.Kind())
	fmt.Printf("reflect.Ptr: (%T) %v\n", reflect.Ptr, reflect.Ptr)

	if inter.Kind() == reflect.Ptr { // 2、判断是不是指针，拿到内部的元素
		inter = inter.Elem()
		fmt.Printf("inter: (%T) %v\n", inter, inter)
	}

	fmt.Printf("inter.Kind(): (%T) %v\n", inter.Kind(), inter.Kind())
	fmt.Printf("reflect.Interface: (%T) %v\n", reflect.Interface, reflect.Interface)
	if inter.Kind() != reflect.Interface {
		panic("this service not interface")
	}

	fmt.Println()
	service := new(userService)
	fmt.Printf("service: (%T) %v\n\n", service, service)

	tp := reflect.TypeOf(service)
	fmt.Printf("tp: (%T) %v\n", tp, tp)

	fmt.Printf("tp.Kind(): (%T) %v\n", tp.Kind(), tp.Kind())
	fmt.Printf("reflect.Ptr: (%T) %v\n", reflect.Ptr, reflect.Ptr)

	if tp.Kind() == reflect.Ptr {
		method := tp.NumMethod() // 获取方法
		for x := 0; x < method; x++ {
			fmt.Printf("%+v\n", tp.Method(x))
		}
		if tp.Implements(inter) { // 判断是否实现了某个接口
			fmt.Printf("%s implatement %s\n\n", tp, inter)
		}
		tp = tp.Elem()
		fmt.Printf("tp: (%T) %v\n", tp, tp)
	}

	fmt.Printf("tp.Kind(): (%T) %v\n", tp.Kind(), tp.Kind())
	fmt.Printf("reflect.Struct: (%T) %v\n", reflect.Struct, reflect.Struct)

	if tp.Kind() == reflect.Struct { //
		fieldN := tp.NumField()
		for x := 0; x < fieldN; x++ { // 获取字段信息
			fmt.Printf("\n%+v\n\n", tp.Field(x))

			fmt.Printf("tp.Field(%d): (%T) %v\n", x, tp.Field(x), tp.Field(x))
			fmt.Printf("tp.Field(x).Type.Kind(): (%T) %v\n", tp.Field(x).Type.Kind(), tp.Field(x).Type.Kind())
			fmt.Printf("reflect.Map: (%T) %v\n", reflect.Map, reflect.Map)
			fmt.Printf("reflect.Array: (%T) %v\n", reflect.Array, reflect.Array)
			if tp.Field(x).Type.Kind() == reflect.Map { // 如果是map，可以获取key元素
				fmt.Printf("FieldName: %s, key: %s.\n", tp.Field(x).Name, tp.Field(x).Type.Key().Kind())
			}
			if tp.Field(x).Type.Kind() == reflect.Array { // 如果是数组，可以获取长度信息
				fmt.Printf("FieldName: %s, len: %d.\n", tp.Field(x).Name, tp.Field(x).Type.Len())
			}
		}
	}
}

/*
in: (*main.UserService) <nil>

inter: (*reflect.rtype) *main.UserService
inter.Kind(): (reflect.Kind) ptr
reflect.Ptr: (reflect.Kind) ptr
inter: (*reflect.rtype) main.UserService
inter.Kind(): (reflect.Kind) interface
reflect.Interface: (reflect.Kind) interface

service: (*main.userService) &{ map[] [0 0 0 0 0 0 0 0 0 0]}

tp: (*reflect.rtype) *main.userService
tp.Kind(): (reflect.Kind) ptr
reflect.Ptr: (reflect.Kind) ptr
{Name:Service PkgPath: Type:func(*main.userService, string) string Func:<func(*main.userService, string) string Value> Index:0}
*main.userService implatement main.UserService

tp: (*reflect.rtype) main.userService
tp.Kind(): (reflect.Kind) struct
reflect.Struct: (reflect.Kind) struct

{Name:ServerName PkgPath: Type:string Tag: Offset:0 Index:[0] Anonymous:false}

tp.Field(0): (reflect.StructField) {ServerName  string  0 [0] false}
tp.Field(x).Type.Kind(): (reflect.Kind) string
reflect.Map: (reflect.Kind) map
reflect.Array: (reflect.Kind) array

{Name:Info PkgPath: Type:map[string]interface {} Tag: Offset:16 Index:[1] Anonymous:false}

tp.Field(1): (reflect.StructField) {Info  map[string]interface {}  16 [1] false}
tp.Field(x).Type.Kind(): (reflect.Kind) map
reflect.Map: (reflect.Kind) map
reflect.Array: (reflect.Kind) array
FieldName: Info, key: string.

{Name:List PkgPath: Type:[10]int Tag: Offset:24 Index:[2] Anonymous:false}

tp.Field(2): (reflect.StructField) {List  [10]int  24 [2] false}
tp.Field(x).Type.Kind(): (reflect.Kind) array
reflect.Map: (reflect.Kind) map
reflect.Array: (reflect.Kind) array
FieldName: List, len: 10.
*/


```



### Value

### Addr 方法

```go
package main

import (
	"fmt"
	"reflect"
)

type UserService interface {
	Service(str string) string
}

type userService struct {
	ServerName string
	Info       map[string]interface{}
	List       [10]int
}

func (*userService) Service(str string) string {
	return "name"
}

func main() {
	ptr_st_value := reflect.ValueOf(&userService{})
	fmt.Printf("ptr_st_value: (%T) %#v\n", ptr_st_value, ptr_st_value)
	st_value := reflect.ValueOf(userService{})
	fmt.Printf("st_value: (%T) %#v\n", st_value, st_value)

	fmt.Println(reflect.ValueOf(&userService{}).CanAddr()) // 所有的 reflect.ValueOf()都不可以直接拿到addr()
	fmt.Println(reflect.ValueOf(userService{}).CanAddr())

	fmt.Println()
	x := 2
	fmt.Printf("x: (%T) %#v\n", x, x)
	d := reflect.ValueOf(&x)
	fmt.Printf("d: (%T) %#v\n", d, d)
	fmt.Printf("d.Elem(): (%T) %v\n", d.Elem(), d.Elem())
	fmt.Printf("d.Elem().CanAddr(): (%T) %v\n", d.Elem().CanAddr(), d.Elem().CanAddr())
	fmt.Printf("d.Elem().Addr(): (%T) %#v\n", d.Elem().Addr(), d.Elem().Addr())
	fmt.Printf("d.Elem().Addr().Interface(): (%T) %#v\n", d.Elem().Addr().Interface(), d.Elem().Addr().Interface())
	value := d.Elem().Addr().Interface().(*int) // 可以直接转换为指针
	fmt.Printf("value: (%T) %#v\n", value, value)
	*value = 1000
	fmt.Printf("*value: (%T) %#v\n", *value, *value)
	fmt.Println(x) // "3"

}

/*
ptr_st_value: (reflect.Value) &main.userService{ServerName:"", Info:map[string]interface {}(nil), List:[10]int{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}}
st_value: (reflect.Value) main.userService{ServerName:"", Info:map[string]interface {}(nil), List:[10]int{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}}
false
false

x: (int) 2
d: (reflect.Value) (*int)(0xc0000be090)
d.Elem(): (reflect.Value) 2
d.Elem().CanAddr(): (bool) true
d.Elem().Addr(): (reflect.Value) (*int)(0xc0000be090)
d.Elem().Addr().Interface(): (*int) (*int)(0xc0000be090)
value: (*int) (*int)(0xc0000be090)
*value: (int) 1000
1000
*/


```



### Set 方法

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {

	fmt.Println()
	x := 2
	fmt.Printf("x: (%T) %#v\n", x, x)
	d := reflect.ValueOf(&x)
	fmt.Printf("d: (%T) %#v\n", d, d)
	fmt.Printf("d.Elem(): (%T) %v\n", d.Elem(), d.Elem())
	fmt.Printf("d.Elem().CanSet(): (%T) %v\n", d.Elem().CanSet(), d.Elem().CanSet())

	d.Elem().SetInt(1000)
	fmt.Printf("x: (%T) %#v\n", x, x)

}

/*
x: (int) 2
d: (reflect.Value) (*int)(0xc00001c100)
d.Elem(): (reflect.Value) 2
d.Elem().CanSet(): (bool) true
x: (int) 1000
*/

```



### Elem

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {

	x := 2
	fmt.Printf("x: (%T) %#v\n", x, x)
	d := reflect.ValueOf(&x)
	fmt.Printf("d: (%T) %#v\n", d, d)
	fmt.Printf("d.Kind(): (%T) %#v\n", d.Kind(), d.Kind())
	fmt.Printf("reflect.Ptr: (%T) %#v\n", reflect.Ptr, reflect.Ptr)
	fmt.Printf("d.Elem(): (%T) %#v\n", d.Elem(), d.Elem())
	if d.Kind() == reflect.Ptr {
		d.Elem() // 调用elem 获取指针真正指向的对象
	}

	// 或者，可以调用这个方法安全的调用
	d = reflect.Indirect(d)
	fmt.Printf("d: (%T) %#v\n", d, d)

}

/*
x: (int) 2
d: (reflect.Value) (*int)(0xc0000a8000)
d.Kind(): (reflect.Kind) 0x16
reflect.Ptr: (reflect.Kind) 0x16
d.Elem(): (reflect.Value) 2
d: (reflect.Value) 2
*/

```



### New & Set (十分重要)

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {

	fmt.Printf("reflect.TypeOf(\"111\"): (%T) %#v\n", reflect.TypeOf("111"), reflect.TypeOf("111"))
	value := reflect.New(reflect.TypeOf("111")) // 初始化一个 string类型的value，但是需要注意的是初始化完成后是 *string，任何类型都是，New()方法调用完成后都会是一个指针指向原来的类型数据，也就是多了个*
	fmt.Printf("value: (%T) %#v\n", value, value)

	// fmt.Println(value.Kind())       // 因此这里输出的是 *string ，ptr
	fmt.Printf("value.Kind(): (%T) %#v\n", value.Kind(), value.Kind())

	value = reflect.Indirect(value) // 获取真正的类型,string，
	fmt.Printf("value: (%T) %#v\n", value, value)
	// fmt.Println(value.Kind())       //
	fmt.Printf("value.Kind(): (%T) %#v\n", value.Kind(), value.Kind())

	if value.CanSet() {
		value.SetString("hello world") // set string,必须要求类型是string的，而且can set，
	}
	fmt.Println(value.Interface().(string)) // "hello world"

}

/*
reflect.TypeOf("111"): (*reflect.rtype) &reflect.rtype{size:0x10, ptrdata:0x8, hash:0xe0ff5cb4, tflag:0x7, align:0x8, fieldAlign:0x8, kind:0x18, equal:(func(unsafe.Pointer, unsafe.Pointer) bool)(0x402d20), gcdata:(*uint8)(0x4b56e8), str:3087, ptrToThis:21760}
value: (reflect.Value) (*string)(0xc00009e210)
value.Kind(): (reflect.Kind) 0x16
value: (reflect.Value) ""
value.Kind(): (reflect.Kind) 0x18
hello world
*/


```

