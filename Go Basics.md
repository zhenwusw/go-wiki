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

Go 语言内存模型

Go 语言内存模型保证同一个变量在不同 goroutine 被读到正确的值。一种情况是变量在一个 goroutine 中被写入以后，另一个 goroutine 能够观测到最新写入的结果。程序中的数据同时被多个 goroutine 访问（包括读和写时），这些并发访问必须以一定的顺序进行。为了将访问序列化，一种方式是采用 channel，一种是使用早期的同步控制方式，比如 sync、sync/atomic 包。

```go
package main

var a string
var c = make(chan int, 10)

func f() {
  a = "hello world"
  c <- 0
}

func main() {
  go f()
  <-c
  print(a)
}

```

像管道的发送操作发生在对应的接收操作结束之前像，所以主线程会阻塞直到对管道 `c` 产生发送操作
