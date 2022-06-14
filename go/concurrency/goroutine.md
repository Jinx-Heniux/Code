# Goroutine

## [Goroutine · Go语言中文文档](https://www.topgoer.com/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B/goroutine.html)

### 启动单个goroutine

```go
package main

import (
	"fmt"
	"time"
)

func hello() {
	fmt.Println("Hello Goroutine!")
}

func main() {
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
	time.Sleep(time.Second)
}

/*
main goroutine done!
Hello Goroutine!
*/

```



### 启动多个goroutine

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func hello(i int) {
	defer wg.Done() // goroutine结束就登记-1
	fmt.Println("Hello Goroutine!", i)
}
func main() {

	for i := 0; i < 10; i++ {
		wg.Add(1) // 启动一个goroutine就登记+1
		go hello(i)
	}
	wg.Wait() // 等待所有登记的goroutine都结束
}

```



### 如果主协程退出了，其他任务还执行吗

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	// 合起来写
	go func() {
		i := 0
		for {
			i++
			fmt.Printf("new goroutine: i = %d\n", i)
			time.Sleep(time.Second)
		}
	}()
	i := 0
	for {
		i++
		fmt.Printf("main goroutine: i = %d\n", i)
		time.Sleep(time.Second)
		if i == 2 {
			break
		}
	}
}

/*
main goroutine: i = 1
new goroutine: i = 1
new goroutine: i = 2
main goroutine: i = 2

or

main goroutine: i = 1
new goroutine: i = 1
main goroutine: i = 2
new goroutine: i = 2
new goroutine: i = 3
*/

```



## [M3.2.1-2v3 | Coursera](https://www.coursera.org/learn/golang-concurrency/lecture/rWw66/m3-2-1-2v3)

```go
package main

import (
	"fmt"
	"sync"
)

func foo(wg *sync.WaitGroup) {
	fmt.Println("new routine")
	wg.Done()
}

func main() {
	var wg sync.WaitGroup
	wg.Add(1)
	go foo(&wg)
	wg.Wait()
	fmt.Println("main routine")
}

```

