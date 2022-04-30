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

