# 11 错误链、哨兵错误与自定义错误

## 学习目标

- 使用 `%w` 包装错误。
- 理解 `errors.Is` 和 `errors.As`。
- 为命令行和 API 提供可分类错误。

## 工程场景

HTTP API 需要把“空输入”映射成 400，把内部读文件失败映射成 500。错误不只是字符串，还承担分类和决策作用。

## 核心概念

包装错误能保留底层原因，同时提供上层上下文。

```go
// textlab/errors.go
package textlab

import "errors"

var ErrEmptyText = errors.New("empty text")
```

```go
// textlab/count.go
package textlab

import (
	"fmt"
	"strings"
)

func CountWordsStrict(text string) (Report, error) {
	if strings.TrimSpace(text) == "" {
		return Report{}, fmt.Errorf("count words: %w", ErrEmptyText)
	}
	return CountWords(text), nil
}
```

## 渐进案例

调用者用 `errors.Is` 判断错误类别。

```go
// main.go
package main

import (
	"errors"
	"fmt"

	"example.com/go-lab/textlab"
)

func main() {
	_, err := textlab.CountWordsStrict("")
	if errors.Is(err, textlab.ErrEmptyText) {
		fmt.Println("please provide text")
		return
	}
	if err != nil {
		fmt.Println("unexpected:", err)
	}
}
```

## 常见坑

- 不要把错误字符串当作稳定 API。
- 包装错误时只有一个 `%w` 表示可解包原因。
- 哨兵错误适合稳定分类，不适合携带大量上下文。

## 练习题

1. 定义 `ErrTooLarge` 并在输入超过限制时返回。
2. 设计 `InputError` 类型，包含字段名和原因。
3. 用 `errors.As` 识别 `InputError`。

## 官方参考链接

- [Package errors](https://pkg.go.dev/errors)
- [Go 1.13 errors blog](https://go.dev/blog/go1.13-errors)
- [Effective Go: Errors](https://go.dev/doc/effective_go#errors)

