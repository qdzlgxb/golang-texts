# 35 Agent 架构与 Provider 抽象

## 学习目标

- 理解 Agent 的最小组成：模型、工具、状态和执行循环。
- 用 Go 接口隔离模型供应商。
- 为 `go-lab` 设计 provider-neutral Agent。

## 工程场景

`go-lab` 已经有文本分析能力。现在让用户用自然语言提出任务，例如“统计这段文本中最常见的 5 个词”，Agent 决定是否调用工具。

## 核心概念

业务代码不应直接依赖某个模型 SDK。先定义稳定领域接口，再为 OpenAI、其他云模型或本地模型提供适配器。

```go
// agent/provider.go
package agent

import "context"

type Message struct {
	Role    string
	Content string
}

type Request struct {
	Messages []Message
	Tools    []ToolSpec
}

type Response struct {
	Message   Message
	ToolCalls []ToolCall
}

type Provider interface {
	Generate(ctx context.Context, req Request) (Response, error)
}
```

## 渐进案例

定义 Agent 只依赖接口。

```go
// agent/agent.go
package agent

import "context"

type Agent struct {
	Provider Provider
	Tools    Registry
}

func (a Agent) Run(ctx context.Context, input string) (string, error) {
	resp, err := a.Provider.Generate(ctx, Request{
		Messages: []Message{{Role: "user", Content: input}},
		Tools:    a.Tools.Specs(),
	})
	if err != nil {
		return "", err
	}
	return resp.Message.Content, nil
}
```

## 常见坑

- 不要让业务层到处出现 provider 专有请求结构。
- 不要把模型输出当作可信指令。
- Agent 循环必须有最大步数、超时和审计。

## 练习题

1. 为 `Provider` 写一个 `FakeProvider`，用于测试。
2. 给 `Agent.Run` 增加最大迭代次数字段。
3. 设计 `Message.Role` 的可选值，并说明如何避免魔法字符串。

## 官方参考链接

- [OpenAI API Docs](https://developers.openai.com/api/docs/)
- [OpenAI Agents SDK](https://developers.openai.com/api/docs/guides/agents)
- [Package context](https://pkg.go.dev/context)

