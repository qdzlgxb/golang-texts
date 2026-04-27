# 07 命令行文本工具 go-lab

## 学习目标

- 把前六章能力组合成一个 CLI 文本工具。
- 使用标准输入读取内容。
- 输出稳定、可测试的结果。

## 工程场景

`go-lab count` 从标准输入读取文本，输出总词数和词频。这是后续 HTTP API、并发任务和 Agent 工具的基础能力。

## 核心概念

命令行工具应遵守清晰的输入输出约定：普通结果写到标准输出，错误写到标准错误，退出码表达成功或失败。

## 渐进案例

```go
// main.go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
	"sort"
	"strings"
)

func readAll(r io.Reader) (string, error) {
	var b strings.Builder
	scanner := bufio.NewScanner(r)
	for scanner.Scan() {
		b.WriteString(scanner.Text())
		b.WriteByte('\n')
	}
	return b.String(), scanner.Err()
}

func countWords(text string) map[string]int {
	counts := map[string]int{}
	for _, word := range strings.Fields(text) {
		counts[strings.ToLower(word)]++
	}
	return counts
}

func run(args []string, in io.Reader, out io.Writer, errOut io.Writer) int {
	if len(args) == 0 || args[0] == "help" {
		fmt.Fprintln(out, "usage: go-lab count < input.txt")
		return 0
	}
	if args[0] != "count" {
		fmt.Fprintln(errOut, "unknown command:", args[0])
		return 2
	}

	text, err := readAll(in)
	if err != nil {
		fmt.Fprintln(errOut, "read input:", err)
		return 1
	}

	counts := countWords(text)
	words := make([]string, 0, len(counts))
	for word := range counts {
		words = append(words, word)
	}
	sort.Strings(words)

	for _, word := range words {
		fmt.Fprintf(out, "%s\t%d\n", word, counts[word])
	}
	return 0
}

func main() {
	os.Exit(run(os.Args[1:], os.Stdin, os.Stdout, os.Stderr))
}
```

## 常见坑

- `bufio.Scanner` 默认 token 上限有限，处理超大行时需要调整 buffer 或使用 `Reader`。
- CLI 输出顺序要稳定，否则测试和脚本集成会困难。
- 错误输出不要混入标准输出，避免破坏管道结果。

## 练习题

1. 增加 `--min-len` 参数，只统计长度达到阈值的词。
2. 增加 `--top N` 参数，只输出出现最多的 N 个词。
3. 增加 `lines` 子命令，统计行数和非空行数。

## 官方参考链接

- [Package flag](https://pkg.go.dev/flag)
- [Package io](https://pkg.go.dev/io)
- [Package bufio](https://pkg.go.dev/bufio)

