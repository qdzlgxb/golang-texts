# 29 GC、内存模型与容量规划

## 学习目标

- 理解 Go GC 的基本目标和成本。
- 掌握内存模型中 happens-before 的核心意义。
- 为高并发 `go-lab` 做容量规划。

## 工程场景

大文本分析会产生大量短生命周期对象。高并发下，分配过多会增加 GC 压力，影响延迟。

## 核心概念

Go GC 是并发垃圾回收器。降低不必要分配、控制对象生命周期、复用缓冲区，都可能改善延迟。内存模型定义了并发读写何时可见。

```go
// buffer.go
package main

import (
	"bytes"
	"sync"
)

var buffers = sync.Pool{
	New: func() any {
		return new(bytes.Buffer)
	},
}

func useBuffer() string {
	buf := buffers.Get().(*bytes.Buffer)
	defer buffers.Put(buf)
	buf.Reset()
	buf.WriteString("go-lab")
	return buf.String()
}
```

## 渐进案例

容量规划从问题开始：

```text
峰值 QPS = 200
平均输入 = 20KB
P95 分析耗时 = 30ms
worker 数 = 16
队列长度 = 200
```

用这些数字估算内存、延迟和拒绝策略，而不是盲目扩大队列。

## 常见坑

- `sync.Pool` 不是缓存，GC 可能清理其中对象。
- 过度复用对象会增加复杂度和数据泄漏风险。
- 不理解 happens-before 时，不要写无锁并发代码。

## 练习题

1. 用 `-benchmem` 找出一个高分配函数。
2. 尝试预分配切片容量并比较分配次数。
3. 用自己的话解释 channel send 和 receive 的同步关系。

## 官方参考链接

- [A Guide to the Go Garbage Collector](https://go.dev/doc/gc-guide)
- [The Go Memory Model](https://go.dev/ref/mem)
- [Package runtime](https://pkg.go.dev/runtime)

