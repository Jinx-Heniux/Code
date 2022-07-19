# 数据格式

## [数据格式 · Go语言中文文档](https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/%E6%95%B0%E6%8D%AE%E6%A0%BC%E5%BC%8F.html)



### 结构体生成json

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Person struct {
	Name  string
	Hobby string
}

func main() {
	p := Person{"5lmh.com", "女"}
	// 编码json
	b, err := json.Marshal(p)
	if err != nil {
		fmt.Println("json err ", err)
	}
	fmt.Printf("b->%v\n%#v\n", b, b)
	fmt.Println(string(b))

	// 格式化输出
	b, err = json.MarshalIndent(p, "", "     ")
	if err != nil {
		fmt.Println("json err ", err)
	}
	fmt.Printf("b->%v\n%#v\n", b, b)
	fmt.Println(string(b))
}

/*
b->[123 34 78 97 109 101 34 58 34 53 108 109 104 46 99 111 109 34 44 34 72 111 98 98 121 34 58 34 229 165 179 34 125]
[]byte{0x7b, 0x22, 0x4e, 0x61, 0x6d, 0x65, 0x22, 0x3a, 0x22, 0x35, 0x6c, 0x6d, 0x68, 0x2e, 0x63, 0x6f, 0x6d, 0x22, 0x2c, 0x22, 0x48, 0x6f, 0x62, 0x62, 0x79, 0x22, 0x3a, 0x22, 0xe5, 0xa5, 0xb3, 0x22, 0x7d}
{"Name":"5lmh.com","Hobby":"女"}
b->[123 10 32 32 32 32 32 34 78 97 109 101 34 58 32 34 53 108 109 104 46 99 111 109 34 44 10 32 32 32 32 32 34 72 111 98 98 121 34 58 32 34 229 165 179 34 10 125]
[]byte{0x7b, 0xa, 0x20, 0x20, 0x20, 0x20, 0x20, 0x22, 0x4e, 0x61, 0x6d, 0x65, 0x22, 0x3a, 0x20, 0x22, 0x35, 0x6c, 0x6d, 0x68, 0x2e, 0x63, 0x6f, 0x6d, 0x22, 0x2c, 0xa, 0x20, 0x20, 0x20, 0x20, 0x20, 0x22, 0x48, 0x6f, 0x62, 0x62, 0x79, 0x22, 0x3a, 0x20, 0x22, 0xe5, 0xa5, 0xb3, 0x22, 0xa, 0x7d}
{
     "Name": "5lmh.com",
     "Hobby": "女"
}
*/

```

### struct tag

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Person struct {
	//"-"是忽略的意思
	Name  string `json:"-"`
	Hobby string `json:"hobby" `
}

func main() {
	p := Person{"5lmh.com", "女"}
	// 编码json
	b, err := json.Marshal(p)
	if err != nil {
		fmt.Println("json err ", err)
	}
	fmt.Printf("b->%v\n%#v\n", b, b)
	fmt.Println(string(b))

	// 格式化输出
	b, err = json.MarshalIndent(p, "", "     ")
	if err != nil {
		fmt.Println("json err ", err)
	}
	fmt.Printf("b->%v\n%#v\n", b, b)
	fmt.Println(string(b))
}

/*
b->[123 34 104 111 98 98 121 34 58 34 229 165 179 34 125]
[]byte{0x7b, 0x22, 0x68, 0x6f, 0x62, 0x62, 0x79, 0x22, 0x3a, 0x22, 0xe5, 0xa5, 0xb3, 0x22, 0x7d}
{"hobby":"女"}
b->[123 10 32 32 32 32 32 34 104 111 98 98 121 34 58 32 34 229 165 179 34 10 125]
[]byte{0x7b, 0xa, 0x20, 0x20, 0x20, 0x20, 0x20, 0x22, 0x68, 0x6f, 0x62, 0x62, 0x79, 0x22, 0x3a, 0x20, 0x22, 0xe5, 0xa5, 0xb3, 0x22, 0xa, 0x7d}
{
     "hobby": "女"
}
*/


```



### 通过map生成json

```go
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	student := make(map[string]interface{})
	student["name"] = "5lmh.com"
	student["age"] = 18
	student["sex"] = "man"
	b, err := json.Marshal(student)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Printf("b->%#v(%T)\n", b, b)
	fmt.Println(string(b))
	fmt.Println(b)
}

/*
b->[]byte{0x7b, 0x22, 0x61, 0x67, 0x65, 0x22, 0x3a, 0x31, 0x38, 0x2c, 0x22, 0x6e, 0x61, 0x6d, 0x65, 0x22, 0x3a, 0x22, 0x35, 0x6c, 0x6d, 0x68, 0x2e, 0x63, 0x6f, 0x6d, 0x22, 0x2c, 0x22, 0x73, 0x65, 0x78, 0x22, 0x3a, 0x22, 0x6d, 0x61, 0x6e, 0x22, 0x7d}([]uint8)
{"age":18,"name":"5lmh.com","sex":"man"}
[123 34 97 103 101 34 58 49 56 44 34 110 97 109 101 34 58 34 53 108 109 104 46 99 111 109 34 44 34 115 101 120 34 58 34 109 97 110 34 125]
*/

```



### 解析到结构体

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Person struct {
	// Age int `json:"age"`
	Age       int    `json:"age,string"`
	Name      string `json:"name"`
	Niubility bool   `json:"niubility"`
}

func main() {
	// 假数据
	b := []byte(`{"age":"18","name":"5lmh.com","marry":false}`)
	// var p Person
	// var p = Person{} 同上
	// p := Person{} 同上
	p := &Person{}
	fmt.Printf("p->%#v\n%p\n", p, p)
	fmt.Printf("&p->%p\n", &p)
	err := json.Unmarshal(b, &p)
	if err != nil {
		fmt.Println(err)
	}
	// fmt.Printf("p->%#v\n", p)
	fmt.Printf("p->%#v\n%p\n", p, p)
	fmt.Printf("&p->%p\n", &p)
	fmt.Println(p)
}

/*
p->&main.Person{Age:0, Name:"", Niubility:false}
0xc0000be000
&p->0xc0000b6018
p->&main.Person{Age:18, Name:"5lmh.com", Niubility:false}
0xc0000be000
&p->0xc0000b6018
&{18 5lmh.com false}
*/


```



### 解析到interface

```go
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	// 假数据
	// int和float64都当float64
	b := []byte(`{"age":1.3,"name":"5lmh.com","marry":false}`)

	// 声明接口
	var i interface{}
	err := json.Unmarshal(b, &i)
	if err != nil {
		fmt.Println(err)
	}
	// 自动转到map
	fmt.Printf("i -> %#v -> %T\n", i, i)
	fmt.Println(i)
	// 可以判断类型
	m := i.(map[string]interface{})
	fmt.Printf("m -> %T,%v\n", m, m)
	for k, v := range m {
		switch vv := v.(type) {
		case float64:
			fmt.Println(k, "是float64类型", vv)
			fmt.Printf("vv -> %T,%v\n", vv, vv)
			fmt.Printf("v -> %T,%v", v, v)
		case string:
			fmt.Println(k, "是string类型", v)
		default:
			fmt.Println("其他")
		}
	}
}

/*
i -> map[string]interface {}{"age":1.3, "marry":false, "name":"5lmh.com"} -> map[string]interface {}
map[age:1.3 marry:false name:5lmh.com]
m -> map[string]interface {},map[age:1.3 marry:false name:5lmh.com]
age 是float64类型 1.3
vv -> float64,1.3
v -> float64,1.3name 是string类型 5lmh.com
其他
*/


```

