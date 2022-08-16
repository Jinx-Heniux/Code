# defer

## [延迟调用（defer） · Go语言中文文档](https://www.topgoer.com/%E5%87%BD%E6%95%B0/%E5%BB%B6%E8%BF%9F%E8%B0%83%E7%94%A8defer.html)

```go
package main

import "fmt"

func main() {

	var whatever [5]struct{}

	fmt.Println(whatever)

	for i := range whatever {
		fmt.Println(i)
	}
}

/*
[{} {} {} {} {}]
0
1
2
3
4
*/

```



```go
package main

import "fmt"

func main() {

	var whatever [5]struct{}

	fmt.Println(whatever)

	for i := range whatever {
		defer fmt.Println(i)
	}
}

/*
[{} {} {} {} {}]
4
3
2
1
0
*/

```



### defer 碰上闭包

```go
package main

import "fmt"

func main() {

	var whatever [5]struct{}

	fmt.Println(whatever)

	for i := range whatever {
		defer func() { fmt.Println(i) }()
	}
}

/*
loop variable i captured by func literal
*/

/*
[{} {} {} {} {}]
4
4
4
4
4
*/

```

