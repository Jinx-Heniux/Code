# 方法集

## [方法集 · Go语言中文文档](https://www.topgoer.com/%E6%96%B9%E6%B3%95/%E6%96%B9%E6%B3%95%E9%9B%86.html)

### 类型 T 方法集包含全部 receiver T 方法

```go
package main

import (
	"fmt"
)

type T struct {
	int
}

func (t T) test() {
	fmt.Println("类型 T 方法集包含全部 receiver T 方法。")
}

func main() {
	t1 := T{1}
	fmt.Printf("t1 is : %v\n", t1)
	t1.test()
	fmt.Println()
	t2 := &t1
	fmt.Printf("t2 is : %v\n", t2)
	t2.test()
}

/*
t1 is : {1}
类型 T 方法集包含全部 receiver T 方法。

t2 is : &{1}
类型 T 方法集包含全部 receiver T 方法。
*/


```



### 类型 \*T 方法集包含全部 receiver T + \*T 方法

```go
package main

import (
	"fmt"
)

type T struct {
	int
}

func (t T) testT() {
	fmt.Println("类型 *T 方法集包含全部 receiver T 方法。")
}

func (t *T) testP() {
	fmt.Println("类型 *T 方法集包含全部 receiver *T 方法。")
}

func main() {
	t1 := T{1}
	t2 := &t1
	fmt.Printf("t1 is : %v\n", t1)
	t1.testT()
	t1.testP()
	fmt.Println()
	fmt.Printf("t2 is : %v\n", t2)
	t2.testT()
	t2.testP()
}

/*
t1 is : {1}
类型 *T 方法集包含全部 receiver T 方法。
类型 *T 方法集包含全部 receiver *T 方法。

t2 is : &{1}
类型 *T 方法集包含全部 receiver T 方法。
类型 *T 方法集包含全部 receiver *T 方法。
*/

```

