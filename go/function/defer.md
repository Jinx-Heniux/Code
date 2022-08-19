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



### defer f.Close

```go
package main

import "fmt"

type Test struct {
	name string
}

func (t *Test) Close() {
	fmt.Println(t.name, " closed")
}
func main() {
	ts := []Test{{"a"}, {"b"}, {"c"}}
	fmt.Printf("%T, %v, %p\n", ts, ts, ts)
	for _, t := range ts {
		fmt.Printf("%T, %v, %p\n", t, t, &t)
		t.Close()
	}
}

/*
[]main.Test, [{a} {b} {c}],0xc00006e150
main.Test, {a}, 0xc000010280
a  closed
main.Test, {b}, 0xc000010280
b  closed
main.Test, {c}, 0xc000010280
c  closed
*/

```



```go
package main

import "fmt"

type Test struct {
	name string
}

func (t *Test) Close() {
	fmt.Println(t.name, " closed")
}
func main() {
	ts := []Test{{"a"}, {"b"}, {"c"}}
	fmt.Printf("%T, %v, %p\n", ts, ts, ts)
	for _, t := range ts {
		fmt.Printf("%T, %v, %p\n", t, t, &t)
		defer t.Close()
	}
}

/*
[]main.Test, [{a} {b} {c}], 0xc00006e150
main.Test, {a}, 0xc000010280
main.Test, {b}, 0xc000010280
main.Test, {c}, 0xc000010280
c  closed
c  closed
c  closed
*/

```



```go
package main

import "fmt"

type Test struct {
	name string
}

func (t *Test) Close() {
	fmt.Println(t.name, " closed")
}
func Close(t Test) {
	fmt.Printf("%T, %v, %p\n", t, t, &t)
	t.Close()
}
func main() {
	ts := []Test{{"a"}, {"b"}, {"c"}}
	fmt.Printf("%T, %v, %p\n", ts, ts, ts)
	for _, t := range ts {
		fmt.Printf("%T, %v, %p\n", t, t, &t)
		defer Close(t)
	}
}

/*
[]main.Test, [{a} {b} {c}], 0xc0000a0150
main.Test, {a}, 0xc00009e240
main.Test, {b}, 0xc00009e240
main.Test, {c}, 0xc00009e240
main.Test, {c}, 0xc00009e2b0
c  closed
main.Test, {b}, 0xc00009e2f0
b  closed
main.Test, {a}, 0xc00009e330
a  closed
*/

```



```go
package main

import "fmt"

type Test struct {
	name string
}

func (t *Test) Close() {
	fmt.Println(t.name, " closed")
}
func main() {
	ts := []Test{{"a"}, {"b"}, {"c"}}
	fmt.Printf("%T, %v, %p\n", ts, ts, ts)
	for _, t := range ts {
		fmt.Printf("%T, %v, %p\n", t, t, &t)
		t2 := t
		fmt.Printf("%T, %v, %p\n", t2, t2, &t2)
		defer t2.Close()
	}
}

/*
[]main.Test, [{a} {b} {c}], 0xc0000a0150
main.Test, {a}, 0xc00009e240
main.Test, {a}, 0xc00009e270
main.Test, {b}, 0xc00009e240
main.Test, {b}, 0xc00009e2d0
main.Test, {c}, 0xc00009e240
main.Test, {c}, 0xc00009e330
c  closed
b  closed
a  closed
*/

```



### 多个 defer 注册，按 FILO 次序执行 ( 先进后出 )。

```go
package main

import "fmt"

func main() {
	test(0)
}

func test(x int) {
	defer fmt.Println("a")
	defer fmt.Println("b")

	defer func() {
		fmt.Println(100 / x)
	}()

	defer fmt.Println("c")
}

/*
c
b
a
panic: runtime error: integer divide by zero
*/

```



### 延迟调用参数在注册时求值或复制，可用指针或闭包 "延迟" 读取。

```go
package main

import "fmt"

func main() {
	test()
}

func test() {
	x, y := 10, 20

	defer func(i int) {
		fmt.Println("defer: ", i, y)
	}(x)

	x += 10
	y += 100

	fmt.Println("x=", x, " y=", y)
}

/*
x= 20  y= 120
defer:  10 120
*/


```

