# Map

## [Map实现原理 · Go语言中文文档](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Map%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86.html)

### Go中Map的使用

```go
package main

import "fmt"

func main() {

	//直接创建初始化一个map
	var mapInit = map[string]string{"xiaoli": "湖南", "xiaoliu": "天津"}
	fmt.Printf("mapInit -> %#v\n", mapInit)
	//声明一个map类型变量,
	//map的key的类型是string，value的类型是string
	var mapTemp map[string]string
	//使用make函数初始化这个变量,并指定大小(也可以不指定)
	mapTemp = make(map[string]string, 10)
	//存储key ，value
	mapTemp["xiaoming"] = "北京"
	mapTemp["xiaowang"] = "河北"
	//根据key获取value,
	//如果key存在，则ok是true，否则是flase
	//v1用来接收key对应的value,当ok是false时，v1是nil
	v1, ok := mapTemp["xiaoming"]
	fmt.Println(ok, v1)
	//当key=xiaowang存在时打印value
	if v2, ok := mapTemp["xiaowang"]; ok {
		fmt.Println(v2)
	}
	//遍历map,打印key和value
	for k, v := range mapTemp {
		fmt.Println(k, v)
	}
	//删除map中的key
	delete(mapTemp, "xiaoming")
	//获取map的大小
	l := len(mapTemp)
	fmt.Println(l)

}

/*
mapInit -> map[string]string{"xiaoli":"湖南", "xiaoliu":"天津"}
true 北京
河北
xiaowang 河北
xiaoming 北京
1
*/

```



## [6、slice和map · 语雀](https://www.yuque.com/aceld/mo95lb/ovmzgh)

### map

```go
package main

import (
	"fmt"
)

func main() {
	//第一种声明
	var test1 map[string]string
	fmt.Println(test1)
	if test1 == nil {
		fmt.Println("test1 is nil")
	}
	//在使用map前，需要先make，make的作用就是给map分配数据空间
	test1 = make(map[string]string, 10)
	test1["one"] = "php"
	test1["two"] = "golang"
	test1["three"] = "java"
	fmt.Println(test1) //map[two:golang three:java one:php]

	//第二种声明
	test2 := make(map[string]string)
	test2["one"] = "php"
	test2["two"] = "golang"
	test2["three"] = "java"
	fmt.Println(test2) //map[one:php two:golang three:java]

	//第三种声明
	test3 := map[string]string{
		"one":   "php",
		"two":   "golang",
		"three": "java",
	}
	fmt.Println(test3) //map[one:php two:golang three:java]

	language := make(map[string]map[string]string)
	language["php"] = make(map[string]string, 2)
	language["php"]["id"] = "1"
	language["php"]["desc"] = "php是世界上最美的语言"
	language["golang"] = make(map[string]string, 2)
	language["golang"]["id"] = "2"
	language["golang"]["desc"] = "golang抗并发非常good"

	fmt.Println(language) //map[php:map[id:1 desc:php是世界上最美的语言] golang:map[id:2 desc:golang抗并发非常good]]

	//增删改查
	// val, key := language["php"]  //查找是否有php这个子元素
	// if key {
	//     fmt.Printf("%v", val)
	// } else {
	//     fmt.Printf("no");
	// }

	//language["php"]["id"] = "3" //修改了php子元素的id值
	//language["php"]["nickname"] = "啪啪啪" //增加php元素里的nickname值
	//delete(language, "php")  //删除了php子元素
	fmt.Println(language)
}

/*
map[]
test1 is nil
map[one:php three:java two:golang]
map[one:php three:java two:golang]
map[one:php three:java two:golang]
map[golang:map[desc:golang抗并发非常good id:2] php:map[desc:php是世界上最美的语言 id:1]]
map[golang:map[desc:golang抗并发非常good id:2] php:map[desc:php是世界上最美的语言 id:1]]
*/


```



```go
package main

import (
	"fmt"
)

func main() {
	//第一种声明
	var test1 map[string]string
	fmt.Println(test1)
	if test1 == nil {
		fmt.Println("test1 is nil")
	}
	//在使用map前，需要先make，make的作用就是给map分配数据空间
	test1 = make(map[string]string, 10)
	test1["one"] = "php"
	test1["two"] = "golang"
	test1["three"] = "java"
	fmt.Println(test1) //map[two:golang three:java one:php]

	//第二种声明
	test2 := make(map[string]string)
	test2["one"] = "php"
	test2["two"] = "golang"
	test2["three"] = "java"
	fmt.Println(test2) //map[one:php two:golang three:java]

	//第三种声明
	test3 := map[string]string{
		"one":   "php",
		"two":   "golang",
		"three": "java",
	}
	fmt.Println(test3) //map[one:php two:golang three:java]

	language := make(map[string]map[string]string)
	language["php"] = make(map[string]string, 2)
	language["php"]["id"] = "1"
	language["php"]["desc"] = "php是世界上最美的语言"
	language["golang"] = make(map[string]string, 2)
	language["golang"]["id"] = "2"
	language["golang"]["desc"] = "golang抗并发非常good"

	fmt.Println(language) //map[php:map[id:1 desc:php是世界上最美的语言] golang:map[id:2 desc:golang抗并发非常good]]

	// 增删改查
	val, key := language["php"] //查找是否有php这个子元素
	if key {
		fmt.Printf("%v\n", val)
	} else {
		fmt.Printf("no\n")
	}

	language["php"]["id"] = "3"         //修改了php子元素的id值
	language["php"]["nickname"] = "啪啪啪" //增加php元素里的nickname值
	delete(language, "php")             //删除了php子元素
	fmt.Println(language)
}

/*
map[]
test1 is nil
map[one:php three:java two:golang]
map[one:php three:java two:golang]
map[one:php three:java two:golang]
map[golang:map[desc:golang抗并发非常good id:2] php:map[desc:php是世界上最美的语言 id:1]]
map[desc:php是世界上最美的语言 id:1]
map[golang:map[desc:golang抗并发非常good id:2]]
*/

```

