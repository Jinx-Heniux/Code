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



### 如类型 S 包含匿名字段 T，则 S 和 \*S 方法集包含 T 方法

```go
package main

import (
	"fmt"
)

type S struct {
	T
}

type T struct {
	int
}

func (t T) testT() {
	fmt.Println("如类型 S 包含匿名字段 T，则 S 和 *S 方法集包含 T 方法。")
}

func main() {
	s1 := S{T{1}}
	s2 := &s1
	fmt.Printf("s1 is : %v\n", s1)
	s1.testT()
	fmt.Printf("s2 is : %v\n", s2)
	s2.testT()
}

/*
s1 is : {{1}}
如类型 S 包含匿名字段 T，则 S 和 *S 方法集包含 T 方法。
s2 is : &{{1}}
如类型 S 包含匿名字段 T，则 S 和 *S 方法集包含 T 方法。
*/

```

