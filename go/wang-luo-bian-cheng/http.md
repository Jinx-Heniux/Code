# HTTP

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



## http编程

[https://www.topgoer.com/%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/http%E7%BC%96%E7%A8%8B.html](https://www.topgoer.com/%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/http%E7%BC%96%E7%A8%8B.html)

### 服务端

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	//http://127.0.0.1:8000/go
	// 单独写回调函数
	http.HandleFunc("/go", myHandler)
	//http.HandleFunc("/ungo",myHandler2 )
	// addr：监听的地址
	// handler：回调函数
	http.ListenAndServe("127.0.0.1:38000", nil)
}

// handler函数
func myHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println(r.RemoteAddr, "连接成功")
	// 请求方式：GET POST DELETE PUT UPDATE
	fmt.Println("method:", r.Method)
	// /go
	fmt.Println("url:", r.URL.Path)
	fmt.Println("header:", r.Header)
	fmt.Println("body:", r.Body)
	// 回复
	w.Write([]byte("www.5lmh.com"))
}

/*
127.0.0.1:50522 连接成功
method: GET
url: /go
header: map[Accept-Encoding:[gzip] User-Agent:[Go-http-client/1.1]]
body: {}
*/

```

### 客户端

```go
package main

import (
	"fmt"
	"io"
	"net/http"
)

func main() {
	//resp, _ := http.Get("http://www.baidu.com")
	//fmt.Println(resp)
	resp, _ := http.Get("http://127.0.0.1:38000/go")
	defer resp.Body.Close()
	// 200 OK
	fmt.Println(resp.Status)
	fmt.Println(resp.Header)

	buf := make([]byte, 1024)
	for {
		// 接收服务端信息
		n, err := resp.Body.Read(buf)
		if err != nil && err != io.EOF {
			fmt.Println(err)
			return
		} else {
			fmt.Println("读取完毕")
			res := string(buf[:n])
			fmt.Println(res)
			break
		}
	}
}

/*
200 OK
map[Content-Length:[12] Content-Type:[text/plain; charset=utf-8] Date:[Tue, 03 May 2022 18:56:20 GMT]]
读取完毕
www.5lmh.com
*/

```



