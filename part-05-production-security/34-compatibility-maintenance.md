# 34 兼容性、版本化与长期维护

## 学习目标

- 理解 Go 1 兼容性承诺的工程意义。
- 设计稳定 API 和版本策略。
- 为 `go-lab` 的 CLI、HTTP 和 SDK 做长期维护规划。

## 工程场景

一旦用户开始脚本化调用 `go-lab`，命令参数、输出格式、HTTP JSON 字段和 SDK 类型都成为兼容性承诺的一部分。

## 核心概念

兼容性不是“不改代码”，而是对公开行为的管理。破坏性变更应通过主版本、迁移指南或兼容层处理。

```text
稳定 API 示例：
- CLI: go-lab count --format json
- HTTP: POST /v1/analyze
- SDK: textlab.CountWords(text string) Report
```

## 渐进案例

为 HTTP API 增加版本前缀：

```go
// routes.go
package main

import "net/http"

func routes() http.Handler {
	mux := http.NewServeMux()
	mux.HandleFunc("POST /v1/analyze", analyzeHandler)
	return mux
}
```

## 常见坑

- 删除 JSON 字段、改变字段含义、改变排序都可能是破坏性变更。
- SDK 的导出类型和字段需要文档化。
- 兼容层也有成本，应定期清理但不能突然移除。

## 练习题

1. 列出 `go-lab` 的公开接口清单。
2. 设计 v1 到 v2 的破坏性变更发布策略。
3. 为废弃字段写一条 deprecation 文档。

## 官方参考链接

- [Go 1 and the Future of Go Programs](https://go.dev/doc/go1compat)
- [Module version numbering](https://go.dev/doc/modules/version-numbers)
- [Developing a major version update](https://go.dev/doc/modules/major-version)

