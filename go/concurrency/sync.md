# sync

## [M3.2.1-2v3 | Coursera](https://www.coursera.org/learn/golang-concurrency/lecture/rWw66/m3-2-1-2v3)

### sync.WaitGroup

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

### sync.Mutex

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

### sync.Once

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

### deadlock

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

func dostuff(c1 chan int, c2 chan int) {
	on.Do(setup)
	fmt.Println("do some stuff...")
	<-c1
	c2 <- 1
	wg.Done()
}

func main() {
	c1 := make(chan int)
	c2 := make(chan int)
	wg.Add(2)
	go dostuff(c1, c2)
	go dostuff(c2, c1)
	wg.Wait()
}

/*
initializing...
do some stuff...
do some stuff...
fatal error: all goroutines are asleep - deadlock!
*/

```

### Dining Philosophers

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

type Chopstick struct{ sync.Mutex }

type Philosopher struct {
	leftCS, rightCS *Chopstick
}

func (p Philosopher) eat() {
	p.leftCS.Lock()
	// time.Sleep(time.Millisecond) // fatal error: all goroutines are asleep - deadlock!
	p.rightCS.Lock()
	fmt.Println("eating...")
	p.rightCS.Unlock()
	p.leftCS.Unlock()
	wg.Done()
}

func main() {
	Chopsticks := make([]*Chopstick, 5)
	fmt.Printf("%#v\n", Chopsticks)

	for i := 0; i < 5; i++ {
		Chopsticks[i] = new(Chopstick)
	}
	fmt.Printf("%#v\n", Chopsticks)

	Philosophers := make([]*Philosopher, 5)
	fmt.Printf("%#v\n", Philosophers)

	for i := 0; i < 5; i++ {
		Philosophers[i] = &Philosopher{
			Chopsticks[i],
			Chopsticks[(i+1)%5],
		}
	}
	fmt.Printf("%#v\n", Philosophers)

	for i := 0; i < 5; i++ {
		wg.Add(1)
		go Philosophers[i].eat()
	}

	wg.Wait()
}

/*
[]*main.Chopstick{(*main.Chopstick)(nil), (*main.Chopstick)(nil), (*main.Chopstick)(nil), (*main.Chopstick)(nil), (*main.Chopstick)(nil)}
[]*main.Chopstick{(*main.Chopstick)(0xc000130000), (*main.Chopstick)(0xc000130008), (*main.Chopstick)(0xc000130010), (*main.Chopstick)(0xc000130018), (*main.Chopstick)(0xc000130020)}
[]*main.Philosopher{(*main.Philosopher)(nil), (*main.Philosopher)(nil), (*main.Philosopher)(nil), (*main.Philosopher)(nil), (*main.Philosopher)(nil)}
[]*main.Philosopher{(*main.Philosopher)(0xc00010c210), (*main.Philosopher)(0xc00010c220), (*main.Philosopher)(0xc00010c230), (*main.Philosopher)(0xc00010c240), (*main.Philosopher)(0xc00010c250)}
eating...
eating...
eating...
eating...
eating...
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



## [并发安全和锁 · Go语言中文文档](https://www.topgoer.com/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B/%E5%B9%B6%E5%8F%91%E5%AE%89%E5%85%A8%E5%92%8C%E9%94%81.html)



```go
package main

import (
	"fmt"
	"sync"
)

var x int64
var wg sync.WaitGroup

func add() {
	for i := 0; i < 5000; i++ {
		x = x + 1
	}
	wg.Done()
}
func main() {
	wg.Add(2)
	go add()
	go add()
	wg.Wait()
	fmt.Println(x)
}

/*
7711

should be 10000
*/

```

### 互斥锁

```go
package main

import (
	"fmt"
	"sync"
)

var x int64
var wg sync.WaitGroup
var lock sync.Mutex

func add() {
	for i := 0; i < 5000; i++ {
		lock.Lock() // 加锁
		x = x + 1
		lock.Unlock() // 解锁
	}
	wg.Done()
}
func main() {
	wg.Add(2)
	go add()
	go add()
	wg.Wait()
	fmt.Println(x)
}

```

### 读写互斥锁

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var (
	x      int64
	wg     sync.WaitGroup
	lock   sync.Mutex
	rwlock sync.RWMutex
)

func write() {
	// lock.Lock()   // 加互斥锁
	rwlock.Lock() // 加写锁
	x = x + 1
	time.Sleep(10 * time.Millisecond) // 假设读操作耗时10毫秒
	rwlock.Unlock()                   // 解写锁
	// lock.Unlock()                     // 解互斥锁
	wg.Done()
}

func read() {
	// lock.Lock()                  // 加互斥锁
	rwlock.RLock()               // 加读锁
	time.Sleep(time.Millisecond) // 假设读操作耗时1毫秒
	rwlock.RUnlock()             // 解读锁
	// lock.Unlock()                // 解互斥锁
	wg.Done()
}

func main() {
	start := time.Now()
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go write()
		fmt.Printf("write: i=%d,x=%d", i, x)
	}

	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go read()
		fmt.Printf("read: i=%d,x=%d", i, x)
	}

	wg.Wait()
	end := time.Now()
	fmt.Printf("\nx=%d\n", x)
	fmt.Println(end.Sub(start))
}

```

