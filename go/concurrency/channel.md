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



### 有缓冲的通道 有缓冲区的通道

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int, 1) // 创建一个容量为1的有缓冲区通道
	ch <- 10
	fmt.Println("发送成功")
}

```



### close()

```go
package main

import "fmt"

func main() {
	c := make(chan int)
	go func() {
		for i := 0; i < 5; i++ {
			c <- i
		}
		//close(c)
	}()
	for {
		if data, ok := <-c; ok {
			fmt.Println(data)
		} else {
			break
		}
	}
	fmt.Println("main结束")
}

/*
0
1
2
3
4
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
main.main()
        /home/zhs2si/go/src/example.jinx.com/hello/goroutines.go:14 +0xbf
exit status 2
*/

// 可以通过内置的close()函数关闭channel（如果你的管道不往里存值或者取值的时候一定记得关闭管道）

```



### 如何优雅的从通道循环取值

```go
package main

import "fmt"

// channel 练习
func main() {
	ch1 := make(chan int)
	ch2 := make(chan int)
	// 开启goroutine将0~100的数发送到ch1中
	go func() {
		for i := 0; i < 11; i++ {
			ch1 <- i
		}
		close(ch1)
	}()
	// 开启goroutine从ch1中接收值，并将该值的平方发送到ch2中
	go func() {
		for {
			i, ok := <-ch1 // 通道关闭后再取值ok=false
			if !ok {
				break
			}
			ch2 <- i * i
		}
		close(ch2)
	}()
	// 在主goroutine中从ch2中接收值打印
	for i := range ch2 { // 通道关闭后会退出for range循环
		fmt.Println(i)
	}
}

/*
0
1
4
9
16
25
36
49
64
81
100
*/

/*
从上面的例子中我们看到有两种方式在接收值的时候判断通道是否被关闭，我们通常使用的是for range的方式。
*/

```



### 单向通道

```go
package main

import "fmt"

func counter(out chan<- int) {
	for i := 0; i < 11; i++ {
		out <- i
	}
	close(out)
}

func squarer(out chan<- int, in <-chan int) {
	for i := range in {
		out <- i * i
	}
	close(out)
}
func printer(in <-chan int) {
	for i := range in {
		fmt.Println(i)
	}
}

func main() {
	ch1 := make(chan int)
	ch2 := make(chan int)
	go counter(ch1)
	go squarer(ch2, ch1)
	printer(ch2)
}

/*
0
1
4
9
16
25
36
49
64
81
100
*/
/*
1.chan<- int是一个只能发送的通道，可以发送但是不能接收；
2.<-chan int是一个只能接收的通道，可以接收但是不能发送。
*/

```



## [2、channel · 语雀](https://www.yuque.com/aceld/mo95lb/ir4ckz)

### channel接收和发送数据都是阻塞的

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	c := make(chan int)

	go func() {
		defer fmt.Println("子go程结束")

		fmt.Println("子go程正在运行……")

		time.Sleep(2 * time.Second)

		c <- 666 //666发送到c
	}()

	fmt.Println("main go程正在运行……")
	num := <-c //从c中接收数据，并赋值给num

	fmt.Println("num = ", num)
	fmt.Println("main go程结束")
}

/*
main go程正在运行……
子go程正在运行……
子go程结束
num =  666
main go程结束
*/


```

