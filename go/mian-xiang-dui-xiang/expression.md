# Expression

## [表达式 · Go语言中文文档](https://www.topgoer.com/%E6%96%B9%E6%B3%95/%E8%A1%A8%E8%BE%BE%E5%BC%8F.html)



```go
package main

import "fmt"

type User struct {
	id   int
	name string
}

func (self *User) Test() {
	fmt.Printf("%p, %v\n", self, self)
}

func main() {
	u := User{1, "Tom"}
	u.Test()

	mValue := u.Test
	fmt.Printf("mValue: %p,%T\n", mValue, mValue)
	mValue() // 隐式传递 receiver

	mExpression := (*User).Test
	fmt.Printf("mExpression: %p,%T\n", mExpression, mExpression)
	mExpression(&u) // 显式传递 receiver
}

/*
0xc0000ac018, &{1 Tom}
mValue: 0x47f760,func()
0xc0000ac018, &{1 Tom}
mExpression: 0x47f460,func(*main.User)
0xc0000ac018, &{1 Tom}
*/


```



### method value 会复制 receiver

```go
package main

import "fmt"

type User struct {
	id   int
	name string
}

func (self User) Test() {
	fmt.Println(self)
}

func main() {
	u := User{1, "Tom"}
	mValue := u.Test // 立即复制 receiver，因为不是指针类型，不受后续修改影响。
	fmt.Printf("mValue: %p,%T\n", mValue, mValue)

	u.id, u.name = 2, "Jack"
	u.Test()

	mValue()
}

/*
mValue: 0x47f920,func()
{2 Jack}
{1 Tom}
*/


```



### method expression，注意 receiver 类型的差异

```go
package main

import "fmt"

type User struct {
	id   int
	name string
}

func (self *User) TestPointer() {
	fmt.Printf("TestPointer: %p, %v\n", self, self)
}

func (self User) TestValue() {
	fmt.Printf("TestValue: %p, %v\n", &self, self)
}

func main() {
	u := User{1, "Tom"}
	fmt.Printf("User u: %p, %v\n", &u, u)
	// u2 := User{2, "Tom2"}
	// fmt.Printf("User u2: %p, %v\n", &u2, u2)

	mv := User.TestValue
	fmt.Printf("mv: %p, %T\n", &mv, mv)
	mv(u)

	mp := (*User).TestPointer
	fmt.Printf("mp: %p, %T\n", &mp, mp)
	mp(&u)

	mp2 := (*User).TestValue // *User 方法集包含 TestValue。签名变为 func TestValue(self *User)。实际依然是 receiver value copy。
	fmt.Printf("mp2: %p, %T\n", &mp2, mp2)
	mp2(&u)
}

/*
User u: 0xc00000c030, {1 Tom}
mv: 0xc00000e030, func(main.User)
TestValue: 0xc00000c060, {1 Tom}
mp: 0xc00000e038, func(*main.User)
TestPointer: 0xc00000c030, &{1 Tom}
mp2: 0xc00000e040, func(*main.User)
TestValue: 0xc00000c0a8, {1 Tom}
*/


```



### 将方法 "还原" 成函数

```go
package main

type Data struct{}

func (Data) TestValue() {}

func (*Data) TestPointer() {}

func main() {
	var p *Data = nil
	p.TestPointer()

	(*Data)(nil).TestPointer() // method value
	(*Data).TestPointer(nil)   // method expression

	// p.TestValue()            // invalid memory address or nil pointer dereference

	// (Data)(nil).TestValue()  // cannot convert nil to type Data
	// Data.TestValue(nil)      // cannot use nil as type Data in function argument
}


```



## [7、面向对象特征 · 语雀](https://www.yuque.com/aceld/mo95lb/aoxo9v)



### 方法

```go
package main

import "fmt"

//定义一个结构体
type T struct {
	name string
}

func (t T) method1() {
	t.name = "new name1"
}

func (t *T) method2() {
	t.name = "new name2"
}

func main() {

	t := T{"old name"}

	fmt.Println("method1 调用前 ", t.name)
	t.method1()
	fmt.Println("method1 调用后 ", t.name)

	fmt.Println("method2 调用前 ", t.name)
	t.method2()
	fmt.Println("method2 调用后 ", t.name)
}

/*
method1 调用前  old name
method1 调用后  old name
method2 调用前  old name
method2 调用后  new name2
*/


```



### 方法值

```go
package main

import (
	"fmt"
	"math"
)

type Point struct{ X, Y float64 }

//这是给struct Point类型定义一个方法
func (p Point) Distance(q Point) float64 {
	return math.Hypot(q.X-p.X, q.Y-p.Y)
}

func main() {

	p := Point{1, 2}
	q := Point{4, 6}

	distanceFormP := p.Distance   // 方法值(相当于C语言的函数地址,函数指针)
	fmt.Println(distanceFormP(q)) // "5"
	fmt.Println(p.Distance(q))    // "5"

	//实际上distanceFormP 就绑定了 p接收器的方法Distance

	distanceFormQ := q.Distance   //
	fmt.Println(distanceFormQ(p)) // "5"
	fmt.Println(q.Distance(p))    // "5"

	//实际上distanceFormQ 就绑定了 q接收器的方法Distance
}

/*
5
5
5
5
*/

```



### 方法表达式

```go
package main

import (
	"fmt"
	"math"
)

type Point struct{ X, Y float64 }

//这是给struct Point类型定义一个方法
func (p Point) Distance(q Point) float64 {
	return math.Hypot(q.X-p.X, q.Y-p.Y)
}

func main() {

	p := Point{1, 2}
	q := Point{4, 6}

	distance1 := Point.Distance //方法表达式, 是一个函数值(相当于C语言的函数指针)
	fmt.Println(distance1(p, q))
	fmt.Printf("%T\n", distance1) //%T表示打出数据类型 ,这个必须放在Printf使用

	distance2 := (*Point).Distance //方法表达式,必须传递指针类型
	distance2(&p, q)
	fmt.Printf("%T\n", distance2)

}

/*
5
func(main.Point, main.Point) float64
func(*main.Point, main.Point) float64
*/

```



### 根据一个变量来决定调用同一个类型的哪个函数

```go
package main

import (
	"fmt"
	"math"
)

type Point struct{ X, Y float64 }

//这是给struct Point类型定义一个方法
func (p Point) Distance(q Point) float64 {
	return math.Hypot(q.X-p.X, q.Y-p.Y)
}

func (p Point) Add(another Point) Point {
	return Point{p.X + another.X, p.Y + another.Y}
}

func (p Point) Sub(another Point) Point {
	return Point{p.X - another.X, p.Y - another.Y}
}

func (p Point) Print() {
	fmt.Printf("{%f, %f}\n", p.X, p.Y)
}

//定义一个Point切片类型 Path
type Path []Point

//方法的接收器 是Path类型数据, 方法的选择器是TranslateBy(Point, bool)
func (path Path) TranslateBy(another Point, add bool) {
	var op func(p, q Point) Point //定义一个 op变量 类型是方法表达式 能够接收Add,和 Sub方法
	// if add == true {
	if add {
		op = Point.Add //给op变量赋值为Add方法
	} else {
		op = Point.Sub //给op变量赋值为Sub方法
	}

	for i := range path {
		//调用 path[i].Add(another) 或者 path[i].Sub(another)
		path[i] = op(path[i], another)
		path[i].Print()
	}
}

func main() {

	points := Path{
		{10, 10},
		{11, 11},
	}

	anotherPoint := Point{5, 5}

	points.TranslateBy(anotherPoint, false)

	fmt.Println("------------------")

	points.TranslateBy(anotherPoint, true)
}

/*
{5.000000, 5.000000}
{6.000000, 6.000000}
------------------
{10.000000, 10.000000}
{11.000000, 11.000000}
*/


```

