# 36 工具调用与安全执行

## 学习目标

- 设计工具描述、输入 schema 和执行接口。
- 将 `textlab` 暴露为 Agent 工具。
- 理解工具调用的权限、超时和审计要求。

## 工程场景

模型可以建议调用 `count_words` 工具，但真正执行必须由 Go 程序控制。工具是能力边界，也是安全边界。

## 核心概念

工具调用至少包含名称、描述、输入结构、执行函数和权限策略。

```go
// agent/tool.go
package agent

import (
	"context"
	"encoding/json"
)

type ToolSpec struct {
	Name        string          `json:"name"`
	Description string          `json:"description"`
	InputSchema json.RawMessage `json:"input_schema"`
}

type ToolCall struct {
	Name      string          `json:"name"`
	Arguments json.RawMessage `json:"arguments"`
}

type Tool interface {
	Spec() ToolSpec
	Call(ctx context.Context, args json.RawMessage) (json.RawMessage, error)
}
```

## 渐进案例

把词频统计注册为工具。

```go
// tools/count_words.go
package tools

import (
	"context"
	"encoding/json"

	"example.com/go-lab/agent"
	"example.com/go-lab/textlab"
)

type CountWordsTool struct{}

type countWordsArgs struct {
	Text string `json:"text"`
}

func (CountWordsTool) Spec() agent.ToolSpec {
	return agent.ToolSpec{
		Name:        "count_words",
		Description: "Count words in provided text.",
		InputSchema: json.RawMessage(`{"type":"object","properties":{"text":{"type":"string"}},"required":["text"]}`),
	}
}

func (CountWordsTool) Call(ctx context.Context, raw json.RawMessage) (json.RawMessage, error) {
	var args countWordsArgs
	if err := json.Unmarshal(raw, &args); err != nil {
		return nil, err
	}
	report := textlab.CountWords(args.Text)
	return json.Marshal(report)
}
```

## 常见坑

- 工具描述来自代码时可信度更高；来自外部服务器时仍应视为不可信。
- 工具输入必须校验大小、格式和权限。
- 高风险工具调用前应要求用户确认。

## 练习题

1. 为工具执行增加 `context.WithTimeout`。
2. 记录工具名、参数大小、耗时和错误类别。
3. 设计只读工具和写操作工具的权限差异。

## 官方参考链接

- [OpenAI Function Calling](https://developers.openai.com/api/docs/guides/function-calling)
- [MCP Tools](https://modelcontextprotocol.io/specification/2025-11-25/server/tools)
- [Package encoding/json](https://pkg.go.dev/encoding/json)
