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

