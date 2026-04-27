# 前言：如何使用这套教材

## 目标读者

这套教材适合三类读者：刚开始学习 Go 的开发者、已经写过脚本或后端但缺少 Go 工程经验的开发者、希望把 Go 用在高并发服务、工具链和 AI Agent/MCP 场景中的工程师。

你不需要预先掌握 Go，但应具备基本命令行使用能力。教材默认你使用 Go 1.26.x，并以模块模式开发，不采用过时的 GOPATH-first 工作流。

## 学习方式

每章都围绕一个工程问题展开。先理解概念，再把概念用于 `go-lab` 的一个小能力。不要只阅读代码；建议你在本地新建自己的练习目录，把章节代码手动输入并运行。

建议节奏：

1. 先通读“学习目标”和“工程场景”。
2. 运行或手写“渐进案例”中的代码。
3. 修改案例，完成练习题。
4. 对照附录中的验收标准检查行为。
5. 再阅读官方参考链接。

## 环境准备

安装 Go 后确认版本：

```sh
go version
```

初始化你的练习模块：

```sh
mkdir go-lab
cd go-lab
go mod init example.com/go-lab
```

教材中的示例模块路径统一写作 `example.com/go-lab`。实际项目中你应改成自己的域名或代码托管路径。

## 关于 AI 与 MCP

AI 章节不把实现绑定到某个供应商。我们会先定义 `Provider`、`Tool`、`Agent` 等 Go 接口，再讨论如何接入具体模型服务。OpenAI、其他云模型或本地模型都可以作为 `Provider` 的实现。

MCP 章节以官方 2025-11-25 规范为准，重点学习协议边界、JSON-RPC 消息、Resources、Prompts、Tools、权限、用户同意和审计，而不是只写一个能跑的 demo。

## 官方参考

- [Go Documentation](https://go.dev/doc/)
- [How to Write Go Code](https://go.dev/doc/code)
- [Go Spec](https://go.dev/ref/spec)
- [Go Modules Reference](https://go.dev/ref/mod)
- [MCP Specification](https://modelcontextprotocol.io/specification/2025-11-25)

