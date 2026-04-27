# 04 数组、切片与映射

## 学习目标

- 区分数组、切片和映射。
- 用切片保存命令参数，用映射统计文本。
- 理解容量、追加和 map 查找惯用法。

## 工程场景

文本工具的第一项真实能力是统计单词出现次数。输入是一段文本，输出是词频表。

## 核心概念

切片是对底层数组的动态视图，map 是哈希表。map 查找通常使用双返回值判断键是否存在。

```go
// main.go
package main

import "fmt"

func main() {
	words := []string{"go", "lab", "go"}
	counts := map[string]int{}

	for _, word := range words {
		counts[word]++
	}

	if n, ok := counts["go"]; ok {
		fmt.Println("go:", n)
	}
}
```

## 渐进案例

使用 `strings.Fields` 对输入分词。

```go
// main.go
package main

import (
	"fmt"
	"strings"
)

func countWords(text string) map[string]int {
	counts := make(map[string]int)
	for _, word := range strings.Fields(text) {
		counts[strings.ToLower(word)]++
	}
	return counts
}

func main() {
	for word, n := range countWords("Go is simple. Go is practical.") {
		fmt.Println(word, n)
	}
}
```

## 常见坑

- map 的遍历顺序不稳定，不要依赖输出顺序。
- nil 切片可以 `append`，nil map 不能直接赋值。
- 切片共享底层数组，传递和截取时要注意意外修改。

## 练习题

1. 过滤长度小于 2 的单词。
2. 将 map 转成切片并按词频排序。
3. 统计总词数和不同单词数量。

## 官方参考链接

- [Go Slices: usage and internals](https://go.dev/blog/slices-intro)
- [Go Spec: Slice types](https://go.dev/ref/spec#Slice_types)
- [Go Spec: Map types](https://go.dev/ref/spec#Map_types)

