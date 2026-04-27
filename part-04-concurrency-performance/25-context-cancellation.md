# 25 context、取消与超时

## 学习目标

- 使用 `context.Context` 传递取消和超时。
- 在 HTTP、数据库和后台任务中尊重取消信号。
- 防止 `go-lab` 任务泄漏。

## 工程场景

用户断开 HTTP 连接后，服务应停止不必要的分析、数据库写入或模型调用。context 是 Go 服务中跨 API 传递生命周期的标准方式。

## 核心概念

context 应作为函数第一个参数传入，不应存进结构体作为长期字段。

```go
// analyze.go
package main

import (
	"context"
	"time"
)

func analyze(ctx context.Context, text string) error {
	select {
	case <-time.After(100 * time.Millisecond):
		return nil
	case <-ctx.Done():
		return ctx.Err()
	}
}
```

## 渐进案例

给任务增加超时。

```go
// main.go
package main

import (
	"context"
	"fmt"
	"time"
)

func main() {
	ctx, cancel := context.WithTimeout(context.Background(), 50*time.Millisecond)
	defer cancel()

	if err := analyze(ctx, "go lab"); err != nil {
		fmt.Println("stopped:", err)
	}
}
```

## 常见坑

- 不要传 `nil` context；不知道用什么时传 `context.Background()`。
- `context.Value` 只适合请求范围元数据，不适合普通函数参数。
- 创建带取消的 context 后应调用 cancel 释放资源。

## 练习题

1. 修改数据库保存函数，接收 `context.Context`。
2. 在 HTTP handler 中把 `r.Context()` 传给业务函数。
3. 写一个测试，验证超时后任务返回 `context.DeadlineExceeded`。

## 官方参考链接

- [Package context](https://pkg.go.dev/context)
- [Canceling in-progress database operations](https://go.dev/doc/database/cancel-operations)
- [Package net/http](https://pkg.go.dev/net/http)

