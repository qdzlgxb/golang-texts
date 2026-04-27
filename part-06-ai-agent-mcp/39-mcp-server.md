# 39 构建 MCP Server

## 学习目标

- 设计最小 MCP Server 结构。
- 暴露 `go-lab` 的 Resources、Prompts 和 Tools。
- 理解能力声明和输入校验。

## 工程场景

把 `go-lab` 作为 MCP Server 后，外部 AI 客户端可以发现 `count_words` 工具、读取分析说明资源、使用提示模板。

## 核心概念

MCP Server 可向 Client 提供 Resources、Prompts、Tools。每一类能力都应有清晰描述和边界。

```go
// mcp/server.go
package mcp

import "context"

type Server struct {
	Tools map[string]Tool
}

type Tool interface {
	Name() string
	Description() string
	Call(ctx context.Context, input []byte) ([]byte, error)
}

func (s Server) ListTools() []map[string]string {
	out := make([]map[string]string, 0, len(s.Tools))
	for _, tool := range s.Tools {
		out = append(out, map[string]string{
			"name":        tool.Name(),
			"description": tool.Description(),
		})
	}
	return out
}
```

## 渐进案例

映射 MCP 方法：

```text
initialize       -> 返回协议版本和 server capabilities
tools/list       -> 返回 count_words
tools/call       -> 校验名称、校验输入、执行工具、返回结果
resources/list   -> 返回 go-lab 使用说明
prompts/list     -> 返回分析提示模板
```

## 常见坑

- 不要把整个本地文件系统作为 resource 暴露。
- 工具 schema 必须和实际解析结构一致。
- Server 端日志不应记录完整敏感输入。

## 练习题

1. 为 `tools/list` 设计响应 JSON。
2. 实现工具不存在时的 JSON-RPC 错误。
3. 设计一个只读 resource：`go-lab://docs/text-analysis`。

## 官方参考链接

- [MCP Server Features](https://modelcontextprotocol.io/specification/2025-11-25/server)
- [MCP Tools](https://modelcontextprotocol.io/specification/2025-11-25/server/tools)
- [MCP Resources](https://modelcontextprotocol.io/specification/2025-11-25/server/resources)

