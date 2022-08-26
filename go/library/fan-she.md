# 反射

## [反射 · Go语言中文文档](https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/%E5%8F%8D%E5%B0%84.html)

### 空接口与反射

#### 反射获取interface类型信息

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


```

