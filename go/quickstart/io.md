# IO

## IO操作 · Go语言中文文档

[https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/IO%E6%93%8D%E4%BD%9C.html](https://www.topgoer.com/%E5%B8%B8%E7%94%A8%E6%A0%87%E5%87%86%E5%BA%93/IO%E6%93%8D%E4%BD%9C.html)

### 打开和关闭文件

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	// 只读方式打开当前目录下的main.go文件
	file, err := os.Open("./main.go")
	if err != nil {
		fmt.Println("open file failed!, err:", err)
		return
	}
	// 关闭文件
	file.Close()
}

// func Open(name string) (file *File, err Error)
// 只读方式打开一个名称为name的文件
```



### 写文件

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	// 新建文件
	file, err := os.Create("./xxx.txt")
	if err != nil {
		fmt.Println(err)
		return
	}
	defer file.Close()
	for i := 0; i < 5; i++ {
		file.WriteString("ab\n")
		file.Write([]byte("cd\n"))
	}

	a := []byte("cd\n")
	fmt.Printf("a -> %#v\n", a)
	fmt.Printf("a -> %v (%T)", a, a)
	/*
		a -> []byte{0x63, 0x64, 0xa}
		a -> [99 100 10] ([]uint8)
	*/
}

// func Create(name string) (file *File, err Error)
// 根据提供的文件名创建新的文件，返回一个文件对象，默认权限是0666

```



### 读文件

```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	// 打开文件
	file, err := os.Open("./xxx.txt")
	if err != nil {
		fmt.Println("open file err :", err)
		return
	}
	defer file.Close()
	// 定义接收文件读取的字节数组
	var buf [128]byte
	var content []byte
	for {
		n, err := file.Read(buf[:])
		// 文件读取可以用file.Read()和file.ReadAt()，读到文件末尾会返回io.EOF的错误
		if err == io.EOF {
			// 读取结束
			break
		}
		if err != nil {
			fmt.Println("read file err ", err)
			return
		}
		content = append(content, buf[:n]...)
	}
	fmt.Println(string(content))
}

/*
func (file *File) Read(b []byte) (n int, err Error)
读取数据到b中
*/

```



### 拷贝文件

```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	// 打开源文件
	srcFile, err := os.Open("./xxx.txt")
	if err != nil {
		fmt.Println(err)
		return
	}
	// 创建新文件
	dstFile, err2 := os.Create("./abc2.txt")
	if err2 != nil {
		fmt.Println(err2)
		return
	}
	// 缓冲读取
	buf := make([]byte, 1024)
	for {
		// 从源文件读数据
		n, err := srcFile.Read(buf)
		if err == io.EOF {
			fmt.Println("读取完毕")
			break
		}
		if err != nil {
			fmt.Println(err)
			break
		}
		//写出去
		dstFile.Write(buf[:n])
	}
	srcFile.Close()
	dstFile.Close()
}

```

