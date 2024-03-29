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



### value.Set()方法set数据的时候，value的类型不能是指针类型

```go
package main

import (
	"fmt"
	"reflect"
	"time"
)

type User struct {
	Name     string
	Age      int
	birthday time.Time //这个不可见
}

func main() {
	user := User{}
	fmt.Printf("user: (%T) %#v\n", user, user)
	value := reflect.ValueOf(&user)
	fmt.Printf("value: (%T) %#v\n", value, value)
	fmt.Println(value.String())
	fmt.Printf("value.String(): (%T) %#v\n", value.String(), value.String())
	for value.Kind() == reflect.Ptr { // 是指针类型，就去拿到真正的类型
		value = value.Elem()
	}
	if value.Kind() == reflect.Struct { // 如果是struct类型，就可以去拿字段
		birthDay := value.FieldByName("birthday") // 这个显然不能set
		fmt.Printf("birthDay: (%T) %#v\n", birthDay, birthDay)
		fmt.Printf("birthDay.CanSet(): (%T) %#v\n", birthDay.CanSet(), birthDay.CanSet())
		if birthDay.CanSet() {
			birthDay.Set(reflect.ValueOf(time.Now()))
		}
		name := value.FieldByName("Name") // 这个可以
		fmt.Printf("name: (%T) %#v\n", name, name)
		fmt.Printf("name.CanSet(): (%T) %#v\n", name.CanSet(), name.CanSet())
		if name.CanSet() {
			name.Set(reflect.ValueOf("tom"))
		}
	}
	fmt.Printf("%+v", user) // {Name:tom Age:0 birthday:{wall:0 ext:0 loc:<nil>}}
}

/*
user: (main.User) main.User{Name:"", Age:0, birthday:time.Time{wall:0x0, ext:0, loc:(*time.Location)(nil)}}
value: (reflect.Value) &main.User{Name:"", Age:0, birthday:time.Time{wall:0x0, ext:0, loc:(*time.Location)(nil)}}
<*main.User Value>
value.String(): (string) "<*main.User Value>"
birthDay: (reflect.Value) time.Time{wall:0x0, ext:0, loc:(*time.Location)(nil)}
birthDay.CanSet(): (bool) false
name: (reflect.Value) ""
name.CanSet(): (bool) true
{Name:tom Age:0 birthday:{wall:0 ext:0 loc:<nil>}}
*/


```



### Call(相当重要)

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
	value := reflect.ValueOf(new(userService))
	fmt.Printf("value: (%T) %v | %#v\n", value, value, value)
	if value.NumMethod() > 0 {
		fmt.Println(value.NumMethod()) // 1
		method := value.MethodByName("Service")
		fmt.Printf("method: (%T) %v | %#v\n", method, method, method)
		fmt.Printf("method.Kind(): (%T) %v | %#v\n", method.Kind(), method.Kind(), method.Kind())
		fmt.Println(method.Kind())                                                  // "func"
		method_call := method.Call([]reflect.Value{reflect.ValueOf("hello world")}) // hello world
		fmt.Printf("method_call: (%T) %v | %#v\n", method_call, method_call, method_call)
	}
}

/*
value: (reflect.Value) &{ map[] [0 0 0 0 0 0 0 0 0 0]} | &main.userService{ServerName:"", Info:map[string]interface {}(nil), List:[10]int{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}}
1
method: (reflect.Value) 0x486500 | (func(string) string)(0x486500)
method.Kind(): (reflect.Kind) func | 0x13
func
method_call: ([]reflect.Value) [name] | []reflect.Value{reflect.Value{typ:(*reflect.rtype)(0x4c7880), ptr:(unsafe.Pointer)(0xc000010340), flag:0x98}}
*/


```



### IsValid

```go
package main

import (
	"fmt"
	"reflect"
)

// isValid 是判断一个value 是不是有值
func main() {
	of := reflect.ValueOf(nil)
	fmt.Printf("of: (%T) %v | %#v\n", of, of, of)
	fmt.Println(of.IsValid()) // false
	fmt.Printf("of.IsValid(): (%T) %v | %#v\n", of.IsValid(), of.IsValid(), of.IsValid())
}

// IsValid reports whether v represents a value. 代表v表示一个值（nil显示不是）
// It returns false if v is the zero Value.（false 是 v是一个空）
/*
value: (reflect.Value) <invalid reflect.Value> | <invalid reflect.Value>
false
of.IsValid(): (bool) false | false
*/


```



### IsNil

```go
package main

import (
	"fmt"
	"reflect"
)

// ok 没问题
func main() {
	i := (*error)(nil)
	fmt.Printf("i: (%T) %v | %#v\n", i, i, i)
	value := reflect.ValueOf(i)
	fmt.Printf("value: (%T) %v | %#v\n", value, value, value)
	fmt.Println(value.IsValid()) // true ,显示不是空
	fmt.Println(value.IsNil())   // true , 因为它确实是nil
}

// IsNil reports whether its argument v is nil. The argument must be
// a chan, func, interface, map, pointer, or slice value; if it is
// not, IsNil panics.

/*
i: (*error) <nil> | (*error)(nil)
value: (reflect.Value) <nil> | (*error)(nil)
true
true
*/


```



```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	var (
		isNilKind = map[reflect.Kind]interface{}{
			reflect.Chan:          nil,
			reflect.Func:          nil,
			reflect.Map:           nil,
			reflect.Ptr:           nil,
			reflect.UnsafePointer: nil,
			reflect.Interface:     nil,
			reflect.Slice:         nil,
		}
	)
	fmt.Printf("isNilKind: (%T) %v | %#v\n", isNilKind, isNilKind, isNilKind)
	value := reflect.ValueOf(1234)
	fmt.Printf("value: (%T) %v | %#v\n", value, value, value)
	fmt.Printf("value.Kind(): (%T) %v | %#v\n", value.Kind(), value.Kind(), value.Kind())
	_, isExist := isNilKind[value.Kind()]
	if isExist {
		fmt.Println(value.IsNil()) // 调用这个需要前置的判断条件，就是kind
	}

}

/*
isNilKind: (map[reflect.Kind]interface {}) map[chan:<nil> func:<nil> interface:<nil> map:<nil> ptr:<nil> slice:<nil> unsafe.Pointer:<nil>] | map[reflect.Kind]interface {}{0x12:interface {}(nil), 0x13:interface {}(nil), 0x14:interface {}(nil), 0x15:interface {}(nil), 0x16:interface {}(nil), 0x17:interface {}(nil), 0x1a:interface {}(nil)}
value: (reflect.Value) 1234 | 1234
value.Kind(): (reflect.Kind) int | 0x2
*/


```



### Field

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
	tp := reflect.TypeOf(new(userService))
	fmt.Printf("tp: (%T) %v | %#v\n", tp, tp, tp)
	fmt.Printf("tp.Kind(): (%T) %v | %#v\n", tp.Kind(), tp.Kind(), tp.Kind())

	if tp.Kind() == reflect.Ptr {
		tp = tp.Elem()
	}

	fmt.Printf("tp: (%T) %v | %#v\n", tp, tp, tp)
	fmt.Printf("tp.Kind(): (%T) %v | %#v\n", tp.Kind(), tp.Kind(), tp.Kind())
	if tp.Kind() != reflect.Struct {
		// t.Fatal("not support")
		panic("not support")
	}

	field, _ := tp.FieldByName("ServerName") // 不许是struct 类型
	fmt.Printf("field: (%T) %v | %#v\n", field, field, field)
	fmt.Printf("FieldTag json=%s\n", field.Tag.Get("json"))
	fmt.Printf("FieldName=%s\n", field.Name)

}

/*
tp: (*reflect.rtype) *main.userService | &reflect.rtype{size:0x8, ptrdata:0x8, hash:0xb4a3a62, tflag:0x9, align:0x8, fieldAlign:0x8, kind:0x36, equal:(func(unsafe.Pointer, unsafe.Pointer) bool)(0x402c20), gcdata:(*uint8)(0x4b8358), str:11640, ptrToThis:0}
tp.Kind(): (reflect.Kind) ptr | 0x16
tp: (*reflect.rtype) main.userService | &reflect.rtype{size:0x68, ptrdata:0x18, hash:0x51ed1e38, tflag:0x7, align:0x8, fieldAlign:0x8, kind:0x19, equal:(func(unsafe.Pointer, unsafe.Pointer) bool)(nil), gcdata:(*uint8)(0x4b77c6), str:11640, ptrToThis:36896}
tp.Kind(): (reflect.Kind) struct | 0x19
field: (reflect.StructField) {ServerName  string  0 [0] false} | reflect.StructField{Name:"ServerName", PkgPath:"", Type:(*reflect.rtype)(0x48a140), Tag:"", Offset:0x0, Index:[]int{0}, Anonymous:false}
FieldTag json=
FieldName=ServerName
*/


```



## [Go语言反射reflect - itbsl - 博客园](https://www.cnblogs.com/itbsl/p/10551880.html)

### IsNil()和IsValid() -- 判断反射值的空和有效性

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {

	//*int的空指针
	var a *int
	fmt.Println("var a *int:", reflect.ValueOf(a).IsNil(), reflect.ValueOf(a).IsValid())

	//nil值
	fmt.Println("nil:", reflect.ValueOf(nil), reflect.ValueOf(nil).IsValid())

	//*int类型的空指针
	fmt.Println("(*int)(nil):", reflect.ValueOf((*int)(nil)).Elem().IsValid())

	//实例化一个结构体
	s := struct{}{}

	//尝试从结构体中查找一个不存在的字段
	fmt.Println("不存在的结构体成员:", reflect.ValueOf(s).FieldByName("").IsValid())

	//尝试从结构体中查找一个不存在的方法
	fmt.Println("不存在的方法:", reflect.ValueOf(s).MethodByName("").IsValid())

	//实例化一个map
	m := map[int]int{}

	//尝试从map中查找一个不存在的键
	fmt.Println("不存在的键:", reflect.ValueOf(m).MapIndex(reflect.ValueOf(3)).IsValid())
}

/*
var a *int: true true
nil: <invalid reflect.Value> false
(*int)(nil): false
不存在的结构体成员: false
不存在的方法: false
不存在的键: false
*/


```

