# 17 go 命令、vet、fmt 与工具链

## 学习目标

- 掌握常用 `go` 子命令。
- 理解 `gofmt`、`go vet`、`go doc` 的角色。
- 建立本地质量检查命令组合。

## 工程场景

工程项目需要稳定的日常命令。每个开发者都应能用同一组命令构建、测试、检查和阅读文档。

## 核心概念

`go` 命令是 Go 工程体验的核心。格式化、测试、构建、安装、文档、环境查询都通过统一工具链完成。

```sh
go fmt ./...
go vet ./...
go test ./...
go doc example.com/go-lab/textlab
go env
```

Go 1.26.x 延续工具链稳定性。教材统一使用模块模式，避免 GOPATH-first 工作流。

## 渐进案例

为 `go-lab` 定义本地检查顺序：

```sh
go fmt ./...
go vet ./...
go test ./...
go test -race ./...
```

## 常见坑

- `go fmt` 会修改文件，提交前应主动运行。
- `go vet` 是静态检查，不等于完整 linter，但能发现重要问题。
- 先让官方工具通过，再考虑引入第三方 lint。

## 练习题

1. 运行 `go env GOMOD GOWORK GOPATH` 并解释输出。
2. 故意写一个 `fmt.Printf("%d", "x")`，观察 `go vet` 报告。
3. 使用 `go doc` 查看 `net/http` 的包文档。

## 官方参考链接

- [Command go](https://pkg.go.dev/cmd/go)
- [Go Doc Comments](https://go.dev/doc/comment)
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)

