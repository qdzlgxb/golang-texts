# 23 Goroutine 与调度直觉

## 学习目标

- 理解 goroutine 是轻量级并发执行单元。
- 建立调度、阻塞和资源成本的基本直觉。
- 为 `go-lab` 的异步分析任务打基础。

## 工程场景

HTTP 请求不应长时间阻塞用户。我们会把文本分析封装为任务，交给后台 goroutine 执行。

## 核心概念

goroutine 由 Go runtime 调度。它比 OS 线程轻量，但不是免费资源。每个 goroutine 都应有明确生命周期。

```go
// main.go
package main

import (
	"fmt"
	"time"
)

func main() {
	go func() {
		fmt.Println("background task")
	}()

	time.Sleep(10 * time.Millisecond)
}
```

## 渐进案例

使用 channel 等待任务完成，而不是随意 sleep。

```go
// main.go
package main

import "fmt"

func main() {
	done := make(chan struct{})

	go func() {
		defer close(done)
		fmt.Println("analyze completed")
	}()

	<-done
}
```

## 常见坑

- 进程退出时未完成的 goroutine 会被直接终止。
- 不要无限创建 goroutine 处理无界任务。
- goroutine 泄漏通常来自无人接收、无人取消或永久阻塞。

## 练习题

1. 启动 10 个 goroutine 分别打印任务编号，并等待全部完成。
2. 故意删除等待逻辑，观察输出不稳定性。
3. 描述 `go-lab` 中哪些任务适合异步执行，哪些不适合。

## 官方参考链接

- [A Tour of Go: Goroutines](https://go.dev/tour/concurrency/1)
- [Effective Go: Goroutines](https://go.dev/doc/effective_go#goroutines)
- [Go Diagnostics](https://go.dev/doc/diagnostics)

