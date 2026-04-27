# 19 结构化日志与 slog

## 学习目标

- 使用标准库 `log/slog` 写结构化日志。
- 区分用户输出和服务日志。
- 为 `go-lab` 增加请求、任务和工具调用日志。

## 工程场景

CLI 输出给用户看，服务日志给工程师排障看。两者不能混在一起。HTTP API 和 Agent 工具执行都需要结构化日志。

## 核心概念

结构化日志用键值对记录事件，便于搜索、聚合和告警。

```go
// main.go
package main

import (
	"log/slog"
	"os"
)

func main() {
	logger := slog.New(slog.NewJSONHandler(os.Stdout, &slog.HandlerOptions{
		Level: slog.LevelInfo,
	}))
	logger.Info("service started", "addr", ":8080")
}
```

## 渐进案例

为文本分析记录耗时。

```go
// analyze.go
package main

import (
	"log/slog"
	"time"
)

func analyze(logger *slog.Logger, text string) {
	start := time.Now()
	words := len(text)
	logger.Info("analyze completed",
		"bytes", words,
		"elapsed_ms", time.Since(start).Milliseconds(),
	)
}
```

## 常见坑

- 不要把密码、token、完整用户隐私文本写入日志。
- 日志字段名应稳定，避免同一含义多种名字。
- 高并发路径中日志过多会影响性能和成本。

## 练习题

1. 增加 `request_id` 字段并贯穿一次处理。
2. 支持文本格式和 JSON 格式日志切换。
3. 设计 Agent 工具调用日志字段：工具名、耗时、结果状态、错误类别。

## 官方参考链接

- [Package log/slog](https://pkg.go.dev/log/slog)
- [Go 1.21 slog blog](https://go.dev/blog/slog)
- [Diagnostics](https://go.dev/doc/diagnostics)

