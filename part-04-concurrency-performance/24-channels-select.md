# 24 Channel、关闭语义与 select

## 学习目标

- 掌握 channel 发送、接收和关闭。
- 理解关闭由发送方或拥有者负责。
- 使用 `select` 处理多个并发事件。

## 工程场景

`go-lab` 的后台任务队列需要接收任务、返回结果，并能在停止时关闭通道。

## 核心概念

channel 用于 goroutine 之间通信。关闭 channel 表示不会再发送新值，不是广播“停止所有事情”的万能开关。

```go
// main.go
package main

import "fmt"

func main() {
	jobs := make(chan string)

	go func() {
		defer close(jobs)
		jobs <- "analyze:1"
		jobs <- "analyze:2"
	}()

	for job := range jobs {
		fmt.Println(job)
	}
}
```

## 渐进案例

使用 `select` 支持结果和取消信号。

```go
// worker.go
package main

import (
	"context"
	"fmt"
)

func waitResult(ctx context.Context, results <-chan string) error {
	select {
	case result := <-results:
		fmt.Println(result)
		return nil
	case <-ctx.Done():
		return ctx.Err()
	}
}
```

## 常见坑

- 向已关闭 channel 发送会 panic。
- 从已关闭 channel 接收会得到零值，需要双返回值判断。
- 多个发送者同时关闭同一个 channel 是常见 bug。

## 练习题

1. 实现一个生产者发送 5 个任务后关闭 channel。
2. 消费者使用 `for range` 读取所有任务。
3. 增加 `select`，在超时后停止等待结果。

## 官方参考链接

- [A Tour of Go: Channels](https://go.dev/tour/concurrency/2)
- [Go Spec: Channel types](https://go.dev/ref/spec#Channel_types)
- [Go Spec: Select statements](https://go.dev/ref/spec#Select_statements)

