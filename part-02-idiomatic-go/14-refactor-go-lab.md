# 14 重构 go-lab 为可测试库

## 学习目标

- 把 CLI 壳和业务核心拆开。
- 使用接口注入输入输出。
- 形成后续 HTTP、并发和 Agent 复用的核心包。

## 工程场景

本章完成第一轮架构整理：`cmd` 层负责参数、I/O、退出码；`textlab` 层负责文本分析；渲染层负责输出格式。

## 核心概念

好的 Go 项目不靠复杂框架组织代码，而靠清晰依赖方向。入口依赖业务包，业务包不反向依赖入口。

建议逻辑结构：

```text
main package -> cli runner -> textlab package
                          -> renderer package
```

## 渐进案例

定义可测试 runner。

```go
// cli/runner.go
package cli

import (
	"fmt"
	"io"

	"example.com/go-lab/textlab"
)

type Runner struct {
	In     io.Reader
	Out    io.Writer
	ErrOut io.Writer
}

func (r Runner) Run(args []string) int {
	if len(args) == 0 || args[0] == "help" {
		fmt.Fprintln(r.Out, "usage: go-lab count")
		return 0
	}
	if args[0] != "count" {
		fmt.Fprintln(r.ErrOut, "unknown command:", args[0])
		return 2
	}

	report, err := textlab.CountFrom(r.In)
	if err != nil {
		fmt.Fprintln(r.ErrOut, err)
		return 1
	}
	fmt.Fprintln(r.Out, report.Counts)
	return 0
}
```

## 常见坑

- `internal` 目录用于限制包导入范围，但不是所有项目都需要马上使用。
- 目录结构应服务依赖边界，而不是照搬模板。
- 重构后必须先补测试，再继续加功能。

## 练习题

1. 为 `Runner.Run` 写测试，使用 `bytes.Buffer` 捕获输出。
2. 把渲染逻辑抽成 `Renderer` 接口。
3. 设计 JSON 输出模式，但暂不实现 HTTP API。

## 官方参考链接

- [Organizing a Go module](https://go.dev/doc/modules/layout)
- [How to Write Go Code](https://go.dev/doc/code)
- [Package bytes](https://pkg.go.dev/bytes)

