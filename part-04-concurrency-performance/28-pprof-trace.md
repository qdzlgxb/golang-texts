# 28 pprof、trace 与性能分析

## 学习目标

- 使用 pprof 分析 CPU、内存、goroutine 和 mutex。
- 使用 trace 观察运行时事件。
- 建立先测量再优化的工作方式。

## 工程场景

当 `go-lab` 在大文本和高并发下变慢时，需要知道时间花在哪里，而不是凭直觉重写代码。

## 核心概念

pprof 适合找热点函数和内存分配，trace 适合观察 goroutine、网络、系统调用和调度事件。

```go
// debug.go
package main

import (
	"net/http"
	_ "net/http/pprof"
)

func serveDebug() error {
	return http.ListenAndServe("localhost:6060", nil)
}
```

基准测试生成 profile：

```sh
go test -run=^$ -bench=. -cpuprofile cpu.out ./...
go tool pprof cpu.out
```

## 渐进案例

给分析函数添加基准测试，并用 `-benchmem` 查看分配：

```sh
go test -bench=CountWords -benchmem ./...
```

## 常见坑

- 不要在没有指标的情况下优化。
- CPU profile、heap profile、mutex profile 会有采样成本。
- 生产开启 debug 端口必须有访问控制。

## 练习题

1. 为 `CountWords` 生成 CPU profile。
2. 用 `-benchmem` 比较 `strings.Fields` 前后的分配。
3. 设计一个只能内网访问的 pprof 暴露方案。

## 官方参考链接

- [Diagnostics](https://go.dev/doc/diagnostics)
- [Profiling Go Programs](https://go.dev/blog/pprof)
- [Package runtime/trace](https://pkg.go.dev/runtime/trace)

