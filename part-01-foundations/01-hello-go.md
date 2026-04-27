# 01 环境、模块与第一个程序

## 学习目标

- 安装并确认 Go 1.26.x 环境。
- 理解 module 是现代 Go 项目的基本边界。
- 写出、运行并构建第一个 Go 程序。

## 工程场景

`go-lab` 从一个最小命令行工具开始。它先只输出版本和帮助信息，后续会逐步扩展成文本处理、HTTP 服务和 Agent 工具平台。

## 核心概念

Go 源码以包为组织单位，入口程序使用 `package main` 和 `func main()`。模块由 `go.mod` 描述，记录模块路径、Go 版本和依赖。

```sh
mkdir go-lab
cd go-lab
go mod init example.com/go-lab
```

```go
// main.go
package main

import "fmt"

func main() {
	fmt.Println("go-lab 0.1.0")
}
```

运行与构建：

```sh
go run .
go build .
```

## 渐进案例

给 `go-lab` 增加一个极简帮助输出。现在先不用第三方 CLI 框架，保持标准库优先。

```go
// main.go
package main

import (
	"fmt"
	"os"
)

func main() {
	if len(os.Args) > 1 && os.Args[1] == "help" {
		fmt.Println("usage: go-lab [help|version]")
		return
	}
	if len(os.Args) > 1 && os.Args[1] == "version" {
		fmt.Println("go-lab 0.1.0")
		return
	}
	fmt.Println("go-lab: try `go-lab help`")
}
```

## 常见坑

- 不要把 GOPATH 当作新项目的默认组织方式；默认使用 module。
- `go run main.go` 只运行指定文件，`go run .` 会按包运行当前目录，更适合多文件程序。
- 模块路径不一定真实存在，但公开模块应使用可解析路径。

## 练习题

1. 增加 `about` 命令，输出工具用途。
2. 当用户传入未知命令时，输出错误信息并用 `os.Exit(1)` 退出。
3. 把版本号提取成包级常量，避免在多个分支重复字符串。

## 官方参考链接

- [Getting started](https://go.dev/doc/tutorial/getting-started)
- [How to Write Go Code](https://go.dev/doc/code)
- [Command go](https://pkg.go.dev/cmd/go)

