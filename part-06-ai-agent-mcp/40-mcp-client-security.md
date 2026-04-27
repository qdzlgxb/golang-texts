# 40 MCP Client、权限与审计

## 学习目标

- 理解 MCP Client 连接和调用 Server 的职责。
- 为工具调用建立用户同意、权限和审计流程。
- 完成 `go-lab` 从 CLI 到 Agent/MCP 工具平台的闭环。

## 工程场景

当 `go-lab` 作为 Host 内部的 MCP Client 连接外部 Server 时，它会看到外部工具描述。描述本身不能被盲目信任，调用前必须校验风险。

## 核心概念

MCP 官方规范强调用户同意、数据隐私、工具安全和 sampling 控制。Client 应把“可调用”和“已授权调用”分开。

```go
// mcp/policy.go
package mcp

import "context"

type Risk string

const (
	RiskLow  Risk = "low"
	RiskHigh Risk = "high"
)

type ToolPolicy struct {
	Name string
	Risk Risk
}

type Authorizer interface {
	Authorize(ctx context.Context, policy ToolPolicy, args []byte) error
}
```

## 渐进案例

调用前执行授权和审计。

```go
// mcp/client.go
package mcp

import (
	"context"
	"log/slog"
)

type Client struct {
	Auth   Authorizer
	Logger *slog.Logger
}

func (c Client) CallTool(ctx context.Context, policy ToolPolicy, args []byte) error {
	if err := c.Auth.Authorize(ctx, policy, args); err != nil {
		c.Logger.Warn("tool denied", "tool", policy.Name, "risk", policy.Risk, "error", err)
		return err
	}
	c.Logger.Info("tool authorized", "tool", policy.Name, "risk", policy.Risk)
	return nil
}
```

## 常见坑

- 不要因为工具来自“已连接服务器”就默认可信。
- Sampling 请求会让 server 间接触发模型调用，必须由用户控制。
- Roots 应限制 server 能访问的 URI 或文件系统边界。

## 练习题

1. 设计高风险工具调用的确认文案和审计字段。
2. 为 `Authorizer` 写一个测试版实现，拒绝所有高风险工具。
3. 描述 MCP Client 如何处理 server 请求 roots、sampling、elicitation。

## 官方参考链接

- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [MCP Roots](https://modelcontextprotocol.io/specification/2025-11-25/client/roots)
- [MCP Sampling](https://modelcontextprotocol.io/specification/2025-11-25/client/sampling)
- [MCP Elicitation](https://modelcontextprotocol.io/specification/2025-11-25/client/elicitation)

