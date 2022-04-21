# Array

{% embed url="https://www.topgoer.com/go%E5%9F%BA%E7%A1%80/%E6%95%B0%E7%BB%84Array.html" %}

```go
package main

import "fmt"

var arr0 [5]int = [5]int{1, 2, 3}
var arr1 = [5]int{1, 2, 3, 4, 5}
var arr2 = [...]int{1, 2, 3, 4, 5, 6}
var str = [5]string{3: "Hello", 4: "Tom"}

func main() {

	a := [3]int{1, 2}
	b := [...]int{1, 2, 3}
	c := [5]int{0: 100, 4: 100}
	d := [...]struct {
		name string
		age  uint8
	}{
		// {name: "user1", age: 10},
		// {name: "user2", age: 20},
		{"user1", 10},
		{"user2", 20},
	}

	fmt.Println(arr0, arr1, arr2, str)
	fmt.Println(a, b, c, d)
}
```

