# 16 Workspaces 与多模块开发

## 学习目标

- 理解 `go.work` 的用途。
- 在多模块本地开发中避免 replace 滥用。
- 为 `go-lab` 规划 app、sdk、tools 的协作方式。

## 工程场景

当 `go-lab` 拆出 `textlab-sdk` 或 MCP 工具包时，多个模块可能需要同时开发。workspace 让本地修改直接参与构建。

## 核心概念

`go.work` 描述一组本地模块。它适合开发环境，不应替代模块本身的依赖声明。

```sh
mkdir app sdk
cd app && go mod init example.com/go-lab/app
cd ../sdk && go mod init example.com/go-lab/sdk
cd ..
go work init ./app ./sdk
go work sync
```

## 渐进案例

将未来 Agent SDK 抽成单独模块时，主应用通过 workspace 使用本地版本：

```text
go-lab/
  go.work
  app/go.mod
  sdk/go.mod
```

本教材不创建这些源码目录，但你应理解这种结构在大型仓库中的价值。

## 常见坑

- 不要把 `go.work` 当作生产部署必需文件。
- workspace 会影响本地解析，排查依赖问题时先确认 `GOWORK`。
- 多模块不是越早越好，只有发布边界不同才值得拆分。

## 练习题

1. 解释 module 和 workspace 的区别。
2. 设计 `go-lab` 拆出 SDK 的模块边界。
3. 说明什么时候使用 `replace`，什么时候使用 `go work`。

## 官方参考链接

- [Tutorial: Getting started with multi-module workspaces](https://go.dev/doc/tutorial/workspaces)
- [Go Modules Reference: Workspaces](https://go.dev/ref/mod#workspaces)
- [Command go: work](https://pkg.go.dev/cmd/go#hdr-Workspace_maintenance)

