# Channel

## [M3.3.1-3v3 | Coursera](https://www.coursera.org/learn/golang-concurrency/lecture/ElArP/m3-3-1-3v3)

```go
package main

import "fmt"

func proc(v1, v2 int, c chan int) {
	c <- v1 * v2
}

func main() {
	c := make(chan int)
	go proc(1, 2, c)
	go proc(3, 4, c)
	a := <-c
	b := <-c
	fmt.Println(a, b, a*b) // 12 2 24
}


```



## [Channel · Go语言中文文档](https://www.topgoer.com/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B/channel.html)

### 创建channel

```go
package main

import "fmt"

func main() {
	var ch1 chan int           // 声明一个传递整型的通道
	var ch2 chan bool          // 声明一个传递布尔型的通道
	var ch3 chan []int         // 声明一个传递int切片的通道
	fmt.Println(ch1, ch2, ch3) // <nil> <nil> <nil>

	ch4 := make(chan int)
	ch5 := make(chan bool)
	ch6 := make(chan []int)
	fmt.Println(ch4, ch5, ch6) // 0xc000022120 0xc000022180 0xc0000221e0

}

```



### 无缓冲的通道又称为阻塞的通道

```go
package main

import "fmt"

func main() {
	ch := make(chan int)
	ch <- 10
	fmt.Println("发送成功")
}

/*
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send]:
main.main()
        /home/zhs2si/go/src/example.jinx.com/hello/goroutines.go:7 +0x31
exit status 2
*/

```



### 无缓冲通道也被称为同步通道

```go
package main

import (
	"fmt"
	"time"
)

func recv(c chan int) {
	ret := <-c
	time.Sleep(time.Second)
	fmt.Println("接收成功", ret)
}
func main() {
	ch := make(chan int)
	go recv(ch) // 启用goroutine从通道接收值
	ch <- 10
	fmt.Println("发送成功")
	time.Sleep(2 * time.Second)
}

/*
发送成功
接收成功 10
*/

```

