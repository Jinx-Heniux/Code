# sync

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



## [M4.2.1-3v3 | Coursera](https://www.coursera.org/learn/golang-concurrency/lecture/asukV/m4-2-1-3v3)

```go
package main

import (
	"fmt"
	"time"
)

var i int = 0

func inc() {
	i = i + 1
}

func main() {
	for j := 0; j < 10000; j++ {
		go inc()
	}
	time.Sleep(time.Second)
	fmt.Println(i)
}
// 9999
// 9476
// 9887
// should be 10000
```



```go
package main

import (
	"fmt"
	"sync"
)

var i int = 0
var mut sync.Mutex
var wg sync.WaitGroup

func inc() {
	mut.Lock()
	i = i + 1
	mut.Unlock()
	wg.Done()
}

func main() {
	for j := 0; j < 10000; j++ {
		wg.Add(1)
		go inc()
	}
	// time.Sleep(time.Second)
	wg.Wait()
	fmt.Println(i)
}

// 10000

```



## [M4.3.1-3v3 | Coursera](https://www.coursera.org/learn/golang-concurrency/lecture/x35sK/m4-3-1-3v3)

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup
var on sync.Once

func setup() {
	fmt.Println("initializing...")
}

func dostuff() {
	on.Do(setup)
	fmt.Println("do some stuff...")
	wg.Done()
}

func main() {
	wg.Add(2)
	go dostuff()
	go dostuff()
	wg.Wait()
}

/*
initializing...
do some stuff...
do some stuff...
*/

```



## [Sync · Go语言中文文档](https://www.topgoer.com/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B/sync.html)

### sync.WaitGroup

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func hello() {
	defer wg.Done()
	fmt.Println("Hello Goroutine!")
}
func main() {
	wg.Add(1)
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
	wg.Wait()
}

/*
main goroutine done!
Hello Goroutine!
*/

```

