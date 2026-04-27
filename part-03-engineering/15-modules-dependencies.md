# 15 Go Modules 与依赖管理

## 学习目标

- 理解 `go.mod`、`go.sum` 和语义化版本。
- 掌握 `go get`、`go mod tidy`、`go list`。
- 为 `go-lab` 建立可复现依赖管理习惯。

## 工程场景

随着 `go-lab` 接入数据库、HTTP 路由或 AI Provider，依赖会增加。依赖管理不只是安装包，还包括版本选择、安全和可重复构建。

## 核心概念

模块是版本化发布单元。`go.mod` 声明直接依赖和 Go 版本，`go.sum` 记录校验和。Go 使用最小版本选择，让构建更可预测。

```sh
go mod init example.com/go-lab
go get github.com/mattn/go-sqlite3@latest
go mod tidy
go list -m all
```

## 渐进案例

为未来数据库章节预留依赖选择原则：

```text
1. 标准库优先：database/sql 提供抽象。
2. 第三方驱动最小化：只引入具体数据库驱动。
3. 依赖升级必须经过测试和漏洞检查。
```

## 常见坑

- 不要手工编辑 `go.sum`。
- `go get` 修改依赖版本，`go install pkg@version` 更适合安装工具。
- 主版本 v2+ 的模块路径通常需要包含 `/v2`。

## 练习题

1. 运行 `go list -m all`，解释每个直接依赖和间接依赖。
2. 尝试升级一个依赖，再用 `go mod tidy` 清理。
3. 查找一个 v2+ 模块，说明它的 import path 为什么包含版本后缀。

## 官方参考链接

- [Go Modules Reference](https://go.dev/ref/mod)
- [Managing dependencies](https://go.dev/doc/modules/managing-dependencies)
- [Module release and versioning workflow](https://go.dev/doc/modules/release-workflow)

