# 38 MCP 协议与 JSON-RPC

## 学习目标

- 理解 MCP 的 Hosts、Clients、Servers 分工。
- 掌握 JSON-RPC 2.0 消息结构。
- 将 Agent 工具边界映射到 MCP 能力。

## 工程场景

`go-lab` 可以作为 MCP Server 暴露文本分析工具，也可以作为 MCP Client 连接外部工具。协议层让能力可组合。

## 核心概念

MCP 是连接 LLM 应用与外部数据、工具、工作流的开放协议。官方 2025-11-25 规范定义了基于 JSON-RPC 2.0 的有状态连接、能力协商和功能集合。

MCP 角色：

```text
Host   : 发起连接的 LLM 应用
Client : Host 内部维护连接的组件
Server : 提供资源、提示和工具的服务
```

JSON-RPC 请求示例：

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/list",
  "params": {}
}
```

## 渐进案例

用 Go 表达 JSON-RPC 基础消息。

```go
// mcp/jsonrpc.go
package mcp

import "encoding/json"

type Request struct {
	JSONRPC string          `json:"jsonrpc"`
	ID      int64           `json:"id,omitempty"`
	Method  string          `json:"method"`
	Params  json.RawMessage `json:"params,omitempty"`
}

type Response struct {
	JSONRPC string          `json:"jsonrpc"`
	ID      int64           `json:"id,omitempty"`
	Result  json.RawMessage `json:"result,omitempty"`
	Error   *Error          `json:"error,omitempty"`
}

type Error struct {
	Code    int    `json:"code"`
	Message string `json:"message"`
}
```

## 常见坑

- 协议消息解析和业务工具执行应分层。
- capability negotiation 决定双方能使用哪些功能，不能跳过。
- MCP 工具可能触发任意代码路径，应有用户同意和权限控制。

## 练习题

1. 为 `Request` 编写 JSON 解码测试。
2. 增加 notification 类型支持，即没有 `id` 的消息。
3. 画出 Host、Client、Server 在 `tools/call` 中的交互顺序。

## 官方参考链接

- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [MCP Architecture](https://modelcontextprotocol.io/specification/2025-11-25/architecture)
- [JSON-RPC 2.0](https://www.jsonrpc.org/specification)

