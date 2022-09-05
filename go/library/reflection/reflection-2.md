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

