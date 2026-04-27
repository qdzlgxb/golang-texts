# 06 错误处理入门

## 学习目标

- 理解 Go 用显式 `error` 表达失败。
- 学会创建、返回和检查错误。
- 让 `go-lab` 对非法输入给出可诊断反馈。

## 工程场景

命令行工具经常遇到缺少参数、文件不存在、输入为空等问题。工程代码不应悄悄忽略这些失败。

## 核心概念

Go 函数常以最后一个返回值返回 `error`。调用者应立即处理错误，决定重试、降级、提示用户或终止。

```go
// main.go
package main

import (
	"errors"
	"fmt"
)

func requireText(text string) error {
	if text == "" {
		return errors.New("text is required")
	}
	return nil
}

func main() {
	if err := requireText(""); err != nil {
		fmt.Println("error:", err)
	}
}
```

## 渐进案例

让词频统计拒绝空文本。

```go
// main.go
package main

import (
	"errors"
	"fmt"
	"strings"
)

func countWords(text string) (map[string]int, error) {
	if strings.TrimSpace(text) == "" {
		return nil, errors.New("empty text")
	}

	counts := map[string]int{}
	for _, word := range strings.Fields(text) {
		counts[strings.ToLower(word)]++
	}
	return counts, nil
}

func main() {
	counts, err := countWords("go go lab")
	if err != nil {
		fmt.Println("error:", err)
		return
	}
	fmt.Println(counts)
}
```

## 常见坑

- 不要用 panic 处理普通业务错误。
- 不要忽略 `err`，尤其是文件、网络、数据库操作。
- 错误消息应说明发生了什么，但不要泄露敏感信息。

## 练习题

1. 当文本超过 1MB 时返回错误。
2. 给错误消息增加参数上下文，例如 `input text is empty`。
3. 修改 `run` 函数，让它根据错误返回不同退出码。

## 官方参考链接

- [Error handling and Go](https://go.dev/blog/error-handling-and-go)
- [Package errors](https://pkg.go.dev/errors)
- [Effective Go: Errors](https://go.dev/doc/effective_go#errors)

