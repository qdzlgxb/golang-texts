# 13 文档、示例与代码风格

## 学习目标

- 编写符合 Go 工具链习惯的文档注释。
- 使用 example 测试展示 API。
- 理解 `gofmt`、命名和小函数的价值。

## 工程场景

当 `textlab` 被 CLI、HTTP API 和 Agent 工具复用时，包文档就是调用者的第一入口。好的文档能减少误用。

## 核心概念

导出标识符应有文档注释，通常以标识符名称开头。示例测试可以出现在文档中，也会被 `go test` 验证。

```go
// textlab/doc.go
// Package textlab provides text analysis helpers for go-lab.
package textlab
```

```go
// textlab/count.go
package textlab

import "strings"

type Report struct {
	Counts map[string]int
}

// CountWords returns a case-insensitive word frequency report.
func CountWords(text string) Report {
	counts := map[string]int{}
	for _, word := range strings.Fields(text) {
		counts[strings.ToLower(word)]++
	}
	return Report{Counts: counts}
}
```

## 渐进案例

添加 example 测试。

```go
// textlab/example_test.go
package textlab_test

import (
	"fmt"

	"example.com/go-lab/textlab"
)

func ExampleCountWords() {
	report := textlab.CountWords("go go lab")
	fmt.Println(report.Counts["go"])
	// Output:
	// 2
}
```

## 常见坑

- 文档不要重复代码表面信息，应解释语义、边界和约定。
- 不要手动对齐代码，交给 `gofmt`。
- 示例输出必须稳定，否则 example 测试会失败。

## 练习题

1. 为 `Report`、`Entry`、`Top` 补充文档注释。
2. 写一个 `ExampleTop`，确保输出顺序稳定。
3. 运行 `go doc` 查看包文档，并改进不清楚的表述。

## 官方参考链接

- [Go Doc Comments](https://go.dev/doc/comment)
- [Effective Go](https://go.dev/doc/effective_go)
- [Package testing: Examples](https://pkg.go.dev/testing#hdr-Examples)
