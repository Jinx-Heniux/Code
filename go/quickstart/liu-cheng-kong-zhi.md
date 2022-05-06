# 流程控制



## 条件语句if

[https://www.topgoer.com/%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/%E6%9D%A1%E4%BB%B6%E8%AF%AD%E5%8F%A5if.html](https://www.topgoer.com/%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/%E6%9D%A1%E4%BB%B6%E8%AF%AD%E5%8F%A5if.html)

### if...else if...else...

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

### if...else...

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



## 条件语句switch

[https://www.topgoer.com/%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/%E6%9D%A1%E4%BB%B6%E8%AF%AD%E5%8F%A5switch.html](https://www.topgoer.com/%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/%E6%9D%A1%E4%BB%B6%E8%AF%AD%E5%8F%A5switch.html)



```go
package main

import "fmt"

func main() {
	/* 定义局部变量 */
	var grade string = "B"
	var marks int = 90

	switch marks {
	case 90:
		grade = "A"
	case 80:
		grade = "B"
	case 50, 60, 70:
		grade = "C"
	default:
		grade = "D"
	}

	switch {
	case grade == "A":
		fmt.Printf("优秀!\n")
	case grade == "B", grade == "C":
		fmt.Printf("良好\n")
	case grade == "D":
		fmt.Printf("及格\n")
	case grade == "F":
		fmt.Printf("不及格\n")
	default:
		fmt.Printf("差\n")
	}
	fmt.Printf("你的等级是 %s\n", grade)
}

```



## 循环语句for

[https://www.topgoer.com/%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/%E5%BE%AA%E7%8E%AF%E8%AF%AD%E5%8F%A5for.html](https://www.topgoer.com/%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6/%E5%BE%AA%E7%8E%AF%E8%AF%AD%E5%8F%A5for.html)



```go
package main

import "fmt"

func main() {
	s := "abc"

	for i, n := 0, len(s); i < n; i++ { // 常见的 for 循环，支持初始化语句。
		fmt.Printf("s[%d]=%c(%d)\n", i, s[i], s[i])
	}
	fmt.Println()

	n := len(s) - 1
	for n >= 0 { // 替代 while (n > 0) {}
		fmt.Printf("s[%d]=%c(%d)\n", n, s[n], s[n]) // 替代 for (; n > 0;) {}
		n--
	}
	fmt.Println()

	// for true {}
	i := 0
	for { // 替代 while (true) {}
		if i == len(s) {
			break
		}
		fmt.Printf("s[%d]=%c(%d)\n", i, s[i], s[i]) // 替代 for (;;) {}
		i++
	}
}

```

