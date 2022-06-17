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
	fmt.Println(a, b, a*b)
}

```



