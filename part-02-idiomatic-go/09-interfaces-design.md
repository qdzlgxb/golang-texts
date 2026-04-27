# 09 接口与依赖反转

## 学习目标

- 理解 Go 接口的隐式实现。
- 学会在消费者侧定义小接口。
- 用接口隔离 I/O 和业务逻辑。

## 工程场景

`go-lab` 需要从标准输入、文件、HTTP 请求甚至 Agent 工具参数中读取文本。核心逻辑不应关心文本来自哪里。

## 核心概念

接口描述行为，不描述继承关系。小接口更容易组合、测试和替换。

```go
// textlab/reader.go
package textlab

import "io"

func CountFrom(r io.Reader) (Report, error) {
	data, err := io.ReadAll(r)
	if err != nil {
		return Report{}, err
	}
	return CountWords(string(data)), nil
}
```

`io.Reader` 是标准库中最重要的小接口之一，只有一个方法，却能连接文件、网络、字符串和缓冲区。

## 渐进案例

使用 `strings.Reader` 测试读取逻辑。

```go
// main.go
package main

import (
	"fmt"
	"strings"

	"example.com/go-lab/textlab"
)

func main() {
	report, err := textlab.CountFrom(strings.NewReader("go lab go"))
	if err != nil {
		panic(err)
	}
	fmt.Println(report.Counts["go"])
}
```

## 常见坑

- 不要先写一个大接口再让所有实现满足它。
- 不要返回接口只为了“抽象”；通常返回具体类型，接收接口。
- `interface{}` 或 `any` 不是泛用逃生口，使用前应明确类型边界。

## 练习题

1. 定义 `Renderer` 接口，负责把 `Report` 输出为文本。
2. 实现一个 TSV renderer。
3. 思考未来 JSON renderer 是否需要修改 `textlab.CountWords`。

## 官方参考链接

- [Go Spec: Interface types](https://go.dev/ref/spec#Interface_types)
- [Effective Go: Interfaces](https://go.dev/doc/effective_go#interfaces_and_types)
- [Package io](https://pkg.go.dev/io)

