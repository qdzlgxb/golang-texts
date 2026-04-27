# 05 结构体、方法与组合

## 学习目标

- 使用结构体表达领域对象。
- 为类型定义方法。
- 理解组合优先于继承的设计方式。

## 工程场景

`go-lab` 需要把词频统计结果、输入来源和处理选项组合成可传递的数据结构。

## 核心概念

结构体是一组命名字段。方法是带接收者的函数。Go 没有类继承，常用结构体嵌入和接口组合实现复用。

```go
// main.go
package main

import "fmt"

type Report struct {
	TotalWords  int
	UniqueWords int
}

func (r Report) Summary() string {
	return fmt.Sprintf("total=%d unique=%d", r.TotalWords, r.UniqueWords)
}

func main() {
	r := Report{TotalWords: 10, UniqueWords: 7}
	fmt.Println(r.Summary())
}
```

## 渐进案例

给词频统计增加报告类型。

```go
// main.go
package main

import (
	"fmt"
	"strings"
)

type WordReport struct {
	Counts map[string]int
}

func (r WordReport) Total() int {
	total := 0
	for _, n := range r.Counts {
		total += n
	}
	return total
}

func NewWordReport(text string) WordReport {
	counts := map[string]int{}
	for _, word := range strings.Fields(text) {
		counts[strings.ToLower(word)]++
	}
	return WordReport{Counts: counts}
}

func main() {
	report := NewWordReport("go go lab")
	fmt.Println(report.Total())
}
```

## 常见坑

- 值接收者会复制接收者；大结构体或需要修改字段时使用指针接收者。
- 不要把所有函数都变成方法；只有和类型行为紧密相关时才定义方法。
- 结构体字段导出后就成为包的公开 API，需要谨慎命名。

## 练习题

1. 为 `WordReport` 增加 `Unique() int` 方法。
2. 增加 `Top(n int) []string`，返回出现最多的单词。
3. 设计 `Input` 结构体，记录来源名称和内容。

## 官方参考链接

- [Go Spec: Struct types](https://go.dev/ref/spec#Struct_types)
- [Go Spec: Method declarations](https://go.dev/ref/spec#Method_declarations)
- [Effective Go: Embedding](https://go.dev/doc/effective_go#embedding)

