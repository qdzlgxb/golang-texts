# 03 控制流与函数

## 学习目标

- 掌握 `if`、`for`、`switch`。
- 使用函数拆分命令解析和执行逻辑。
- 理解多返回值在错误处理中的作用。

## 工程场景

`go-lab` 的命令数量会增加。把所有判断堆在 `main` 中会很快失控，需要用函数把解析、分发和业务逻辑拆开。

## 核心概念

Go 只有一种循环关键字 `for`。`switch` 默认不贯穿执行，不需要每个分支写 `break`。

```go
// main.go
package main

import "fmt"

func classify(command string) string {
	switch command {
	case "help", "version":
		return "builtin"
	case "count":
		return "text"
	default:
		return "unknown"
	}
}

func main() {
	for _, cmd := range []string{"help", "count", "bad"} {
		fmt.Println(cmd, classify(cmd))
	}
}
```

## 渐进案例

把命令执行拆成 `run` 函数，为后续测试做准备。

```go
// main.go
package main

import (
	"fmt"
	"os"
)

func run(args []string) int {
	if len(args) == 0 {
		fmt.Println("usage: go-lab [help|version]")
		return 0
	}

	switch args[0] {
	case "help":
		fmt.Println("usage: go-lab [help|version]")
		return 0
	case "version":
		fmt.Println("go-lab 0.1.0")
		return 0
	default:
		fmt.Println("unknown command:", args[0])
		return 1
	}
}

func main() {
	os.Exit(run(os.Args[1:]))
}
```

## 常见坑

- `range` 返回的是索引和值；不需要索引时用 `_`。
- `if err := do(); err != nil {}` 中的 `err` 只在 `if` 作用域内有效。
- 过早使用匿名函数会降低可读性；先用具名函数表达意图。

## 练习题

1. 给 `run` 增加 `about` 命令。
2. 写函数 `isBuiltin(command string) bool`。
3. 让无参数时输出帮助并返回退出码 `0`，未知命令返回 `2`。

## 官方参考链接

- [Go Spec: For statements](https://go.dev/ref/spec#For_statements)
- [Go Spec: Switch statements](https://go.dev/ref/spec#Switch_statements)
- [Effective Go: Control structures](https://go.dev/doc/effective_go#if)

