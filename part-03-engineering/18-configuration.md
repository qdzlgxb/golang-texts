# 18 配置、环境变量与命令行参数

## 学习目标

- 使用 `flag` 解析命令行参数。
- 区分配置来源的优先级。
- 让 `go-lab` 支持端口、日志级别和输入限制。

## 工程场景

CLI 和服务端程序都需要配置。合理的配置策略应简单、可解释、可测试。

## 核心概念

常见优先级：命令行参数高于环境变量，高于默认值。不要在业务逻辑深处直接读取环境变量，应集中解析。

```go
// config.go
package main

import (
	"flag"
	"os"
)

type Config struct {
	Addr     string
	LogLevel string
}

func LoadConfig(args []string) Config {
	cfg := Config{
		Addr:     ":8080",
		LogLevel: "info",
	}
	if v := os.Getenv("GO_LAB_ADDR"); v != "" {
		cfg.Addr = v
	}
	fs := flag.NewFlagSet("go-lab", flag.ContinueOnError)
	fs.StringVar(&cfg.Addr, "addr", cfg.Addr, "listen address")
	fs.StringVar(&cfg.LogLevel, "log-level", cfg.LogLevel, "log level")
	_ = fs.Parse(args)
	return cfg
}
```

## 渐进案例

未来 HTTP 服务使用同一个 `Config`，避免 CLI 和服务配置分叉。

```go
// main.go
package main

import "fmt"

func main() {
	cfg := LoadConfig([]string{"-addr", ":9090"})
	fmt.Println(cfg.Addr)
}
```

## 常见坑

- 配置解析失败不能静默忽略。
- 敏感配置不要写入日志。
- 默认值应适合本地开发，但生产环境必须显式配置关键项。

## 练习题

1. 增加 `MaxInputBytes int64` 配置。
2. 当 flag 解析失败时返回错误，而不是忽略。
3. 写表驱动测试覆盖默认值、环境变量和 flag 覆盖。

## 官方参考链接

- [Package flag](https://pkg.go.dev/flag)
- [Package os](https://pkg.go.dev/os)
- [Command go: environment](https://pkg.go.dev/cmd/go#hdr-Environment_variables)

