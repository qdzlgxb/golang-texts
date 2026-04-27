# 10 泛型与约束

## 学习目标

- 理解类型参数和约束。
- 判断何时该用泛型，何时保持简单函数。
- 为 `go-lab` 编写可复用排序辅助逻辑。

## 工程场景

词频报告需要按词频、名称或长度排序。泛型可以抽取“取前 N 个元素”等与具体元素类型无关的逻辑。

## 核心概念

泛型让函数和类型对一组类型工作。约束描述类型参数必须满足的能力。

```go
// slicesx/top.go
package slicesx

func FirstN[T any](items []T, n int) []T {
	if n < 0 {
		n = 0
	}
	if n > len(items) {
		n = len(items)
	}
	return items[:n]
}
```

## 渐进案例

定义词频条目，再复用 `FirstN`。

```go
// textlab/top.go
package textlab

import (
	"sort"

	"example.com/go-lab/slicesx"
)

type Entry struct {
	Word  string
	Count int
}

func Top(counts map[string]int, n int) []Entry {
	entries := make([]Entry, 0, len(counts))
	for word, count := range counts {
		entries = append(entries, Entry{Word: word, Count: count})
	}
	sort.Slice(entries, func(i, j int) bool {
		if entries[i].Count == entries[j].Count {
			return entries[i].Word < entries[j].Word
		}
		return entries[i].Count > entries[j].Count
	})
	return slicesx.FirstN(entries, n)
}
```

## 常见坑

- 不要用泛型替代清晰的领域类型。
- 约束越复杂，读者理解成本越高。
- 泛型函数仍应有明确行为和边界测试。

## 练习题

1. 为 `FirstN` 写表驱动测试用例。
2. 实现 `Map[T, U any](items []T, fn func(T) U) []U`。
3. 比较泛型 `Map` 和普通 `for` 循环在可读性上的差异。

## 官方参考链接

- [Tutorial: Getting started with generics](https://go.dev/doc/tutorial/generics)
- [Go Spec: Type parameters](https://go.dev/ref/spec#Type_parameters)
- [Effective Go](https://go.dev/doc/effective_go)

