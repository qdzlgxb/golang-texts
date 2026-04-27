# 33 可观测性与生产诊断

## 学习目标

- 区分日志、指标、追踪和 profile。
- 为服务端 `go-lab` 设计诊断入口。
- 学会用证据定位生产问题。

## 工程场景

用户报告“分析变慢”时，你需要知道是 CPU、数据库、锁竞争、队列堆积还是模型调用变慢。可观测性让问题可定位。

## 核心概念

Go 官方诊断资料把工具分为 profiling、tracing、debugging、runtime statistics and events。生产中应按问题选择工具，而不是一次性打开所有开销。

```go
// health.go
package main

import (
	"encoding/json"
	"net/http"
)

func healthz(w http.ResponseWriter, r *http.Request) {
	_ = json.NewEncoder(w).Encode(map[string]string{
		"status": "ok",
	})
}
```

## 渐进案例

建议的服务诊断面：

```text
GET /healthz        活性检查
GET /readyz         依赖检查
/debug/pprof/*      受保护的性能诊断
结构化日志          请求、任务、工具调用
指标                QPS、延迟、错误率、队列长度
```

## 常见坑

- 健康检查不应执行昂贵逻辑。
- readiness 和 liveness 语义不同。
- trace id、request id、job id 应能串联日志和请求。

## 练习题

1. 为 `go-lab` 设计 5 个核心指标。
2. 给每个分析任务生成 job id 并写入日志。
3. 描述一次 P95 延迟升高的排查流程。

## 官方参考链接

- [Diagnostics](https://go.dev/doc/diagnostics)
- [Package net/http/pprof](https://pkg.go.dev/net/http/pprof)
- [Package runtime/metrics](https://pkg.go.dev/runtime/metrics)

