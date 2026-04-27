# 26 Worker Pool、限流与背压

## 学习目标

- 实现固定数量 worker。
- 理解限流和背压。
- 控制 `go-lab` 并发分析任务的资源占用。

## 工程场景

如果每个请求都启动一个无限制后台任务，服务会在高峰期耗尽 CPU、内存或数据库连接。worker pool 用固定并发保护系统。

## 核心概念

worker pool 用任务 channel 分发工作，用结果 channel 或回调返回结果。队列满时应明确拒绝、等待或降级。

```go
// pool.go
package main

import (
	"context"
	"fmt"
	"sync"
)

type Job struct {
	ID   int
	Text string
}

func startWorkers(ctx context.Context, n int, jobs <-chan Job) {
	var wg sync.WaitGroup
	for i := 0; i < n; i++ {
		wg.Add(1)
		go func(workerID int) {
			defer wg.Done()
			for {
				select {
				case job, ok := <-jobs:
					if !ok {
						return
					}
					fmt.Println(workerID, job.ID)
				case <-ctx.Done():
					return
				}
			}
		}(i)
	}
	wg.Wait()
}
```

## 渐进案例

提交任务时使用有界队列。

```go
// queue.go
package main

import "errors"

var ErrQueueFull = errors.New("queue full")

func submit(jobs chan<- Job, job Job) error {
	select {
	case jobs <- job:
		return nil
	default:
		return ErrQueueFull
	}
}
```

## 常见坑

- 无界队列会把压力转移到内存。
- worker 数量不是越多越好，要按 CPU、I/O 和外部依赖能力设置。
- 队列满时必须有产品语义：返回 429、排队或丢弃。

## 练习题

1. 实现一个 3 worker 的分析池。
2. 当队列满时让 HTTP API 返回 429。
3. 增加简单限流：每秒最多接受 N 个任务。

## 官方参考链接

- [Effective Go: Channels](https://go.dev/doc/effective_go#channels)
- [Package sync](https://pkg.go.dev/sync)
- [Go Concurrency Patterns](https://go.dev/blog/pipelines)

