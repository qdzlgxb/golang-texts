# 30 构建、交付与部署

## 学习目标

- 构建可发布的 Go 二进制。
- 理解交叉编译和构建信息。
- 为 `go-lab` 制定基础部署策略。

## 工程场景

`go-lab` 既可以作为 CLI，也可以作为服务端二进制部署。Go 的静态编译体验让交付更简单，但仍需关注版本、配置和运行环境。

## 核心概念

构建产物应可追踪版本、提交、构建时间和依赖信息。

```sh
go build -o go-lab .
go version -m ./go-lab
```

交叉编译示例：

```sh
GOOS=linux GOARCH=amd64 go build -o go-lab-linux-amd64 .
```

Windows PowerShell：

```powershell
$env:GOOS="linux"; $env:GOARCH="amd64"; go build -o go-lab-linux-amd64 .
```

## 渐进案例

使用 `-ldflags` 注入版本：

```go
// version.go
package main

var version = "dev"
```

```sh
go build -ldflags="-X main.version=0.1.0" .
```

## 常见坑

- 运行环境的配置不应写死在二进制中。
- 容器 CPU 限制会影响 Go runtime 的并发行为，应结合 Go 1.26.x 的 runtime 行为测试。
- 不要把 debug 端口无鉴权暴露到公网。

## 练习题

1. 构建本机二进制并使用 `go version -m` 查看信息。
2. 为 `go-lab version` 输出构建版本。
3. 设计 CLI 和服务端两种部署方式的配置清单。

## 官方参考链接

- [Command go: build](https://pkg.go.dev/cmd/go#hdr-Compile_packages_and_dependencies)
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go Release History](https://go.dev/doc/devel/release)

