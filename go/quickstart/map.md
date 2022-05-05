# Map

## Map · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Map.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Map.html)

### map基本使用

```go
package main
import "fmt"

func main() {
    scoreMap := make(map[string]int, 8)
    fmt.Println(scoreMap, len(scoreMap))
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    fmt.Println(scoreMap, len(scoreMap))
    fmt.Println(scoreMap["小明"])
    fmt.Printf("type of a:%T\n", scoreMap)
}
```



```go
package main
import "fmt"

func main() {
    userInfo := map[string]string{
        "username": "pprof.cn",
        "password": "123456",
    }
    fmt.Println(userInfo)
}
```

### 判断某个键是否存在

```go
package main

import "fmt"

func main() {
	scoreMap := make(map[string]int)
	scoreMap["张三"] = 90
	scoreMap["小明"] = 100
	// 如果key存在ok为true,v为对应的值；不存在ok为false,v为值类型的零值
	v1, ok1 := scoreMap["张三"]
	if ok1 {
		fmt.Println(v1)
	} else {
		fmt.Println("查无此人")
	}
	v2, ok2 := scoreMap["张"]
	if ok2 {
		fmt.Println(v2)
	} else {
		fmt.Println("查无此人")
		fmt.Println(v2)
	}
}

/*
90
查无此人
0
*/

```

### map的遍历

使用for range遍历map

```go
package main
import "fmt"

func main() {
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    scoreMap["王五"] = 60
    for k, v := range scoreMap {
        fmt.Println(k, v)
    }
}
```

只想遍历key

```go
package main
import "fmt"

func main() {
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    scoreMap["王五"] = 60
    for k := range scoreMap {
        fmt.Print(k)
        fmt.Printf(" -> %d\n", scoreMap[k])
    }
}
// 注意： 遍历map时的元素顺序与添加键值对的顺序无关。

```



### 使用delete()函数删除键值对

```go
package main

import "fmt"

func main() {
	scoreMap := make(map[string]int)
	scoreMap["张三"] = 90
	scoreMap["小明"] = 100
	scoreMap["王五"] = 60
	fmt.Println(scoreMap)
	delete(scoreMap, "小明") //将小明:100从map中删除
	for k, v := range scoreMap {
		fmt.Println(k, v)
	}
}

/*
map[小明:100 张三:90 王五:60]
王五 60
张三 90
*/

```

### 按照指定顺序遍历map

```go
package main
import (
	"fmt"
	"math/rand"
	"sort"
	"time"
)

func main() {
    rand.Seed(time.Now().UnixNano()) //初始化随机数种子

    var scoreMap = make(map[string]int, 200)

    for i := 0; i < 100; i++ {
        key := fmt.Sprintf("stu%04d", i) //生成stu开头的字符串
        value := rand.Intn(100)          //生成0~99的随机整数
        scoreMap[key] = value
    }
    
    for k, v := range scoreMap{
        fmt.Printf("%s -> %d\n",k,v)
    }
    
    fmt.Println("=====================")
    
    //取出map中的所有key存入切片keys
    var keys = make([]string, 0, 200)
    for key := range scoreMap {
        keys = append(keys, key)
    }
    //对切片进行排序
    sort.Strings(keys)
    //按照排序后的key遍历map
    for _, key := range keys {
        fmt.Println(key, scoreMap[key])
    }
}
```



### 元素为map类型的切片

```go
package main

import "fmt"

func main() {
	var mapSlice = make([]map[string]string, 3)
	for index, value := range mapSlice {
		fmt.Printf("index:%d value:%v || %#v\n", index, value, value)
		fmt.Println(value == nil)
	}
	fmt.Println("================after init============")
	// 对切片中的map元素进行初始化
	mapSlice[0] = make(map[string]string, 10)
	mapSlice[0]["name"] = "王五"
	mapSlice[0]["password"] = "123456"
	mapSlice[0]["address"] = "红旗大街"
	for index, value := range mapSlice {
		fmt.Printf("index:%d value:%v\n", index, value)
		fmt.Println(value == nil)
	}
}

/*
index:0 value:map[] || map[string]string(nil)
true
index:1 value:map[] || map[string]string(nil)
true
index:2 value:map[] || map[string]string(nil)
true
================after init============
index:0 value:map[address:红旗大街 name:王五 password:123456]
false
index:1 value:map[]
true
index:2 value:map[]
true
*/

```



### 值为切片类型的map

```go
package main

import "fmt"

func main() {
	var sliceMap = make(map[string][]string, 3)
	fmt.Println(sliceMap)
	fmt.Println("after init")
	key := "中国"
	value, ok := sliceMap[key]
	fmt.Println(value)
	fmt.Println(value == nil)
	if !ok {
		value = make([]string, 0, 2)
	}
	value = append(value, "北京", "上海")
	sliceMap[key] = value
	fmt.Println(sliceMap)
}

/*
map[]
after init
[]
true
map[中国:[北京 上海]]
*/

```



## Map实现原理 · Go语言中文文档

[https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Map%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86.html](https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/Map%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86.html)

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

