# 08 包、可见性与项目边界

## 学习目标

- 理解包是 Go 代码复用和封装的基本单位。
- 掌握大小写控制导出规则。
- 为 `go-lab` 设计可测试的包边界。

## 工程场景

`main.go` 中已经混合了命令解析、输入读取和词频统计。下一步要把核心逻辑拆到包里，让 CLI、HTTP API 和 Agent 工具都能复用。

## 核心概念

Go 以标识符首字母大小写控制可见性。大写导出，小写仅包内可见。包名应短小、明确，避免 `utils`、`common` 这类失去边界感的名字。

```go
// textlab/count.go
package textlab

import "strings"

type Report struct {
	Counts map[string]int
}

func CountWords(text string) Report {
	counts := map[string]int{}
	for _, word := range strings.Fields(text) {
		counts[strings.ToLower(word)]++
	}
	return Report{Counts: counts}
}
```

## 渐进案例

CLI 入口只负责 I/O 和退出码，业务包负责纯计算。

```go
// main.go
package main

import (
	"fmt"

	"example.com/go-lab/textlab"
)

func main() {
	report := textlab.CountWords("go go lab")
	fmt.Println(report.Counts)
}
```

## 常见坑

- 包名不需要和目录路径的最后一段完全一致，但通常应保持一致。
- 不要为了测试导出内部函数；优先测试公开行为。
- 包之间出现循环依赖通常说明边界设计错误。

## 练习题

1. 将词频统计移动到 `textlab` 包。
2. 只导出 `Report` 和 `CountWords`，保持辅助函数私有。
3. 画出 CLI、`textlab`、未来 HTTP API 之间的依赖方向。

## 官方参考链接

- [How to Write Go Code](https://go.dev/doc/code)
- [Go Spec: Packages](https://go.dev/ref/spec#Packages)
- [Effective Go: Package names](https://go.dev/doc/effective_go#package-names)

