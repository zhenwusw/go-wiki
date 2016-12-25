Go Basics

包初始化

Go 语言会在程序真正执行前对整个程序的依赖进行分析，并初始化相关代码包。所有的代码包初始化函数都会在 `main`函数之前执行完成，而且只会执行一次。并且，对于每一个代码包来说，其中所有全部变量的初始化都会在改代码包的初始化行数执行前完成。

```go
package main
import (
  "fmt"
  "runtime"
)
func inti() {  // 包初始化函数
  fmt.Printf("xxxx")
}
var m map[int]string = map[int]string{1: "A", 2: "B", 3: "C"}
var info string

func main() {
  fmt.Println(info)
}
```

