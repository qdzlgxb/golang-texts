# 12 测试、表驱动与基准测试

## 学习目标

- 编写单元测试和表驱动测试。
- 理解基准测试的用途。
- 用测试保护 `go-lab` 的核心文本逻辑。

## 工程场景

词频统计看似简单，但大小写、空白、标点和排序都可能引入回归。测试是重构前的安全网。

## 核心概念

Go 测试文件以 `_test.go` 结尾。测试函数形如 `func TestXxx(t *testing.T)`，基准测试形如 `func BenchmarkXxx(b *testing.B)`。

```go
// textlab/count_test.go
package textlab

import "testing"

func TestCountWords(t *testing.T) {
	got := CountWords("Go go lab").Counts
	if got["go"] != 2 {
		t.Fatalf("go count = %d, want 2", got["go"])
	}
}
```

## 渐进案例

表驱动测试覆盖多个输入。

```go
// textlab/count_test.go
package textlab

import "testing"

func TestCountWordsTable(t *testing.T) {
	tests := []struct {
		name string
		text string
		word string
		want int
	}{
		{name: "lowercase", text: "go go", word: "go", want: 2},
		{name: "mixed case", text: "Go go", word: "go", want: 2},
		{name: "missing", text: "go", word: "lab", want: 0},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			got := CountWords(tt.text).Counts[tt.word]
			if got != tt.want {
				t.Fatalf("count = %d, want %d", got, tt.want)
			}
		})
	}
}
```

基准测试：

```go
// textlab/count_test.go
package textlab

import "testing"

func BenchmarkCountWords(b *testing.B) {
	text := "go lab go concurrency service agent"
	for i := 0; i < b.N; i++ {
		_ = CountWords(text)
	}
}
```

## 常见坑

- 测试不要只覆盖快乐路径。
- 基准测试中不要把准备数据的成本混入被测逻辑，必要时使用 `b.ResetTimer()`。
- 不稳定输出会让测试脆弱，因此 map 输出前应排序。

## 练习题

1. 为 `Top` 写表驱动测试。
2. 增加空文本、中文文本和多空格输入测试。
3. 编写 `BenchmarkTop`，比较不同输入规模。

## 官方参考链接

- [Package testing](https://pkg.go.dev/testing)
- [Add a test](https://go.dev/doc/tutorial/add-a-test)
- [Go Wiki: TableDrivenTests](https://go.dev/wiki/TableDrivenTests)

