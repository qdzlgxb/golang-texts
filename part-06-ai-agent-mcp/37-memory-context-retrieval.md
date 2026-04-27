# 37 记忆、上下文与检索

## 学习目标

- 区分会话上下文、长期记忆和外部检索。
- 设计可控的上下文注入流程。
- 避免把敏感数据无边界交给模型。

## 工程场景

用户可能让 Agent 分析之前上传过的文本，或者基于历史分析结果提问。`go-lab` 需要检索相关上下文，但不能把全部数据库记录都塞进 prompt。

## 核心概念

上下文管理的核心是选择、压缩和授权。Go 侧应控制哪些数据进入模型请求，并记录来源。

```go
// agent/context.go
package agent

import "context"

type Document struct {
	ID      string
	Title   string
	Snippet string
	Source  string
}

type Retriever interface {
	Search(ctx context.Context, query string, limit int) ([]Document, error)
}
```

## 渐进案例

将检索结果转成系统上下文。

```go
// agent/prompt.go
package agent

import "strings"

func BuildContextMessage(docs []Document) Message {
	var b strings.Builder
	for _, doc := range docs {
		b.WriteString("- ")
		b.WriteString(doc.Title)
		b.WriteString(": ")
		b.WriteString(doc.Snippet)
		b.WriteByte('\n')
	}
	return Message{Role: "system", Content: b.String()}
}
```

## 常见坑

- 长上下文不是越多越好，会增加成本、延迟和误导风险。
- 不能把权限外数据因为“检索相关”就注入模型。
- 模型生成的引用需要能追溯到真实来源。

## 练习题

1. 实现一个内存版 `Retriever`。
2. 为检索结果增加 `OwnerID` 并检查权限。
3. 设计上下文压缩策略：超过 5 条结果时如何裁剪。

## 官方参考链接

- [OpenAI Retrieval](https://developers.openai.com/api/docs/guides/retrieval)
- [MCP Resources](https://modelcontextprotocol.io/specification/2025-11-25/server/resources)
- [Package context](https://pkg.go.dev/context)

