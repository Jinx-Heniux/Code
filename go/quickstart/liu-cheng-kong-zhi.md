# 流程控制



## 条件语句if

[https://www.topgoer.com/%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/%E6%9D%A1%E4%BB%B6%E8%AF%AD%E5%8F%A5if.html](https://www.topgoer.com/%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/%E6%9D%A1%E4%BB%B6%E8%AF%AD%E5%8F%A5if.html)



```go
package main

func main() {
	x := 1
	// x := -1
	// x := 0

	if n := "abc"; x > 0 { // 初始化语句未必就是定义变量， 如 println("init") 也是可以的。
		println(n[2])
	} else if x < 0 { // 注意 else if 和 else 左大括号位置。
		println(n[1])
	} else {
		println(n[0])
	}
}

```



```go
package main

import "fmt"

func main() {
	/* 定义局部变量 */
	var a int = 10
	/* 使用 if 语句判断布尔表达式 */
	if a < 20 {
		/* 如果条件为 true 则执行以下语句 */
		fmt.Printf("a 小于 20\n")
	}
	fmt.Printf("a 的值为 : %d\n", a)
}

```



```go
package main

import "fmt"

func main() {
	/* 局部变量定义 */
	var a int = 100
	/* 判断布尔表达式 */
	if a < 20 {
		/* 如果条件为 true 则执行以下语句 */
		fmt.Printf("a 小于 20\n")
	} else {
		/* 如果条件为 false 则执行以下语句 */
		fmt.Printf("a 不小于 20\n")
	}
	fmt.Printf("a 的值为 : %d\n", a)

}

```



```go
package main

import "fmt"

func main() {
	/* 定义局部变量 */
	var a int = 100
	var b int = 200
	/* 判断条件 */
	if a == 100 {
		/* if 条件语句为 true 执行 */
		if b == 200 {
			/* if 条件语句为 true 执行 */
			fmt.Printf("a 的值为 100 ， b 的值为 200\n")
		}
	}
	fmt.Printf("a 值为 : %d\n", a)
	fmt.Printf("b 值为 : %d\n", b)
}

```

