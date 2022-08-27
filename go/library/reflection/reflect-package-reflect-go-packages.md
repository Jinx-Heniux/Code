# reflect package - reflect - Go Packages

[https://pkg.go.dev/reflect](https://pkg.go.dev/reflect)

## func TypeOf

```go
package main

import (
	"fmt"
	"io"
	"os"
	"reflect"
)

func main() {
	// As interface types are only used for static typing, a
	// common idiom to find the reflection Type for an interface
	// type Foo is to use a *Foo value.
	writerType := reflect.TypeOf((*io.Writer)(nil))
	fmt.Printf("%T, %v\n", writerType, writerType)
	writerType = writerType.Elem()
	fmt.Printf("%T, %v\n", writerType, writerType)

	fileType := reflect.TypeOf((*os.File)(nil))
	fmt.Printf("%T, %v\n", fileType, fileType)

	fmt.Println(fileType.Implements(writerType))

}

/*
*reflect.rtype, *io.Writer
*reflect.rtype, io.Writer
*reflect.rtype, *os.File
true
*/

```

