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
    delete(scoreMap, "小明")//将小明:100从map中删除
    for k,v := range scoreMap{
        fmt.Println(k, v)
    }
}
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

