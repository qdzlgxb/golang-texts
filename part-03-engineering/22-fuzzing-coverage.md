# 22 Fuzzing、Coverage 与质量门禁

## 学习目标

- 使用 fuzzing 发现边界输入问题。
- 使用 coverage 观察测试覆盖范围。
- 将质量检查纳入 `go-lab` 日常开发。

## 工程场景

文本分析会面对任意用户输入，包括空白、超长文本、Unicode、非法编码和奇怪标点。fuzzing 可以自动生成边界样本。

## 核心概念

Fuzz 测试以 `FuzzXxx` 命名，使用种子输入和随机生成输入探索问题。Coverage 不是质量本身，但能提示没有被测试触达的区域。

```go
// textlab/count_fuzz_test.go
package textlab

import "testing"

func FuzzCountWords(f *testing.F) {
	f.Add("go go lab")
	f.Add("")
	f.Add("你好 Go")

	f.Fuzz(func(t *testing.T, text string) {
		report := CountWords(text)
		if report.Counts == nil {
			t.Fatalf("Counts is nil")
		}
	})
}
```

运行：

```sh
go test -fuzz=FuzzCountWords -fuzztime=30s ./...
go test -cover ./...
```

## 渐进案例

为 `go-lab` 设置本地质量门禁：

```sh
go test ./...
go test -race ./...
go test -cover ./...
go vet ./...
```

## 常见坑

- fuzzing 发现失败样本后，应把它保留下来变成回归测试。
- 覆盖率高不代表断言有效。
- 不要让 fuzz 测试依赖网络、时间或不可控外部系统。

## 练习题

1. 为 `Top` 编写 fuzz 测试，确保不会越界。
2. 生成 coverage 报告并指出一个未覆盖函数。
3. 设计 CI 中 fuzzing 的短运行策略。

## 官方参考链接

- [Fuzzing](https://go.dev/doc/security/fuzz/)
- [Tutorial: Getting started with fuzzing](https://go.dev/doc/tutorial/fuzz)
- [Coverage for Go applications](https://go.dev/doc/build-cover)

