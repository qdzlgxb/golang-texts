# 02 值、变量与类型

## 学习目标

- 掌握基本类型、零值、短变量声明和常量。
- 理解静态类型如何帮助工程维护。
- 在 `go-lab` 中处理版本、路径和布尔配置。

## 工程场景

CLI 工具需要保存版本号、输入文件路径、是否启用详细输出等配置。正确建模这些值能减少隐式错误。

## 核心概念

Go 变量声明可以显式写类型，也可以让编译器推断。每种类型都有零值，未初始化变量也处于可用状态。

```go
// main.go
package main

import "fmt"

const version = "0.1.0"

func main() {
	var input string
	verbose := false
	count := 0

	fmt.Println(version, input, verbose, count)
}
```

常用基础类型包括 `bool`、`string`、整数、浮点数、`byte`、`rune`。不要为了“省事”把所有东西都存成字符串；类型本身就是约束。

## 渐进案例

定义 `Config` 的雏形，用明确字段描述命令行状态。

```go
// main.go
package main

import "fmt"

const version = "0.1.0"

type Config struct {
	Command string
	Input   string
	Verbose bool
	Limit   int
}

func main() {
	cfg := Config{
		Command: "version",
		Limit:   100,
	}
	fmt.Printf("%+v\n", cfg)
}
```

## 常见坑

- `:=` 只能在函数内部使用。
- 未使用变量会导致编译失败，这是 Go 维护代码整洁性的设计。
- 类型别名和新定义类型不同；需要表达领域含义时优先使用新类型。

## 练习题

1. 给 `Config` 增加 `Output string` 字段。
2. 定义 `type Command string`，并为 `version`、`help`、`count` 定义常量。
3. 写一个函数 `defaultConfig() Config` 返回默认配置。

## 官方参考链接

- [Go Spec: Variables](https://go.dev/ref/spec#Variables)
- [Go Spec: Types](https://go.dev/ref/spec#Types)
- [A Tour of Go: Basics](https://go.dev/tour/basics/1)

