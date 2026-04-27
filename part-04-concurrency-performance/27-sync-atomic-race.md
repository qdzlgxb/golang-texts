# 27 sync、atomic 与竞态检测

## 学习目标

- 使用 `sync.Mutex`、`sync.RWMutex`、`sync.WaitGroup`。
- 理解 `sync/atomic` 的适用范围。
- 使用 race detector 发现数据竞态。

## 工程场景

`go-lab` 需要统计任务数量、成功数量和失败数量。多个 worker 同时更新共享状态时必须同步。

## 核心概念

只要多个 goroutine 访问同一变量，且至少一个是写操作，就需要同步。不要用“看起来不会同时发生”作为并发安全依据。

```go
// stats.go
package main

import "sync"

type Stats struct {
	mu      sync.Mutex
	Success int
	Failed  int
}

func (s *Stats) IncSuccess() {
	s.mu.Lock()
	defer s.mu.Unlock()
	s.Success++
}
```

## 渐进案例

简单计数器可用 atomic。

```go
// counter.go
package main

import "sync/atomic"

type Counter struct {
	value atomic.Int64
}

func (c *Counter) Inc() {
	c.value.Add(1)
}

func (c *Counter) Load() int64 {
	return c.value.Load()
}
```

运行竞态检测：

```sh
go test -race ./...
```

## 常见坑

- `WaitGroup.Add` 应在启动 goroutine 前调用。
- atomic 适合简单数值和指针操作，不适合复杂不变量。
- map 不是并发写安全的。

## 练习题

1. 写一个没有锁的并发计数器，用 `-race` 观察报告。
2. 用 `sync.Mutex` 修复它。
3. 比较 mutex 和 atomic 在表达复杂状态时的可读性。

## 官方参考链接

- [Package sync](https://pkg.go.dev/sync)
- [Package sync/atomic](https://pkg.go.dev/sync/atomic)
- [Data Race Detector](https://go.dev/doc/articles/race_detector)

