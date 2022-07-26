# select

## [select · Go语言中文文档](https://www.topgoer.com/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B/select.html)

### select可以同时监听一个或多个channel，直到其中一个channel ready

```go
package main

import (
	"fmt"
	"time"
)

func test1(ch chan string) {
	time.Sleep(time.Second * 5)
	ch <- "test1"
}
func test2(ch chan string) {
	time.Sleep(time.Second * 2)
	ch <- "test2"
}

func main() {
	// 2个管道
	output1 := make(chan string)
	output2 := make(chan string)
	// 跑2个子协程，写数据
	go test1(output1)
	go test2(output2)
	// 用select监控
	select {
	case s1 := <-output1:
		fmt.Println("s1=", s1)
	case s2 := <-output2:
		fmt.Println("s2=", s2)
	}
}

/*
s2= test2
*/

```

### 如果多个channel同时ready，则随机选择一个执行

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	// 创建2个管道
	int_chan := make(chan int, 1)
	string_chan := make(chan string, 1)
	go func() {
		//time.Sleep(2 * time.Second)
		time.Sleep(1 * time.Millisecond)
		int_chan <- 1
	}()
	go func() {
		time.Sleep(2 * time.Millisecond)
		string_chan <- "hello"
	}()
	select {
	case value := <-int_chan:
		fmt.Println("int:", value)
	case value := <-string_chan:
		fmt.Println("string:", value)
	}
	fmt.Println("main结束")
}

/*
string: hello
main结束

or
int: 1
main结束
*/

```

### 可以用于判断管道是否存满

```go
package main

import (
	"fmt"
	"time"
)

// 判断管道有没有存满
func main() {
	// 创建管道
	output1 := make(chan string, 10)
	// 子协程写数据
	go write(output1)
	// 取数据
	for s := range output1 {
		fmt.Println("res:", s)
		time.Sleep(time.Second)
	}
}

func write(ch chan string) {
	for {
		select {
		// 写数据
		case ch <- "hello":
			fmt.Println("write hello")
		default:
			fmt.Println("channel full")
		}
		time.Sleep(time.Millisecond * 500)
	}
}

// 21 write hello
// 11 res: hello
/*
write hello
res: hello
write hello
write hello
res: hello
write hello
res: hello
write hello
write hello
res: hello
write hello
write hello
res: hello
write hello
write hello
write hello
res: hello
write hello
res: hello
write hello
write hello
res: hello
write hello
write hello
res: hello
write hello
write hello
res: hello
write hello
write hello
res: hello
write hello
channel full
*/

```



```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int, 10)

	go write(ch)

	for r := range ch {
		fmt.Printf("recv: %d\n", r)
		time.Sleep(time.Second)
	}
}

func write(ch chan int) {
	for {
		select {
		case ch <- 0:
			fmt.Println("write 0")
		default:
			fmt.Println("channel full")
		}
		time.Sleep(time.Millisecond * 500)
	}

}

/*
write 0
recv: 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
write 0
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
channel full
recv: 0
write 0
*/


```

