# net/http

## Http · Go语言中文文档

[https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/http.html](https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/http.html)

### GET请求示例

使用net/http包编写一个简单的发送HTTP请求的Client端

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func main() {
	// resp, err := http.Get("https://www.5lmh.com/")
	resp, err := http.Get("http://www.5lmh.com/")
	if err != nil {
		fmt.Println("get failed, err:", err)
		return
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println("read from resp.Body failed,err:", err)
		return
	}
	fmt.Print(string(body))
}

// get failed, err: Get "https://www.5lmh.com/":
// x509: certificate is valid for topgoer.com, not www.5lmh.com

```



