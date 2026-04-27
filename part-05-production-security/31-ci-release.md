# 31 CI、测试矩阵与发布流程

## 学习目标

- 设计 Go 项目的 CI 检查。
- 理解测试矩阵、缓存和发布标签。
- 为 `go-lab` 制定可重复发布流程。

## 工程场景

当 `go-lab` 成为团队项目，不能依赖每个人手动运行检查。CI 应在合并前执行关键质量门禁。

## 核心概念

基础 CI 至少包含格式、vet、测试、race 和漏洞检查中的关键项。发布流程应从 tag 触发构建，并保留构建产物。

```yaml
# .github/workflows/ci.yml
name: ci
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: "1.26.x"
      - run: go test ./...
      - run: go vet ./...
      - run: go test -race ./...
```

## 渐进案例

发布前本地检查：

```sh
go mod tidy
go test ./...
go test -race ./...
go vet ./...
govulncheck ./...
git tag v0.1.0
```

## 常见坑

- CI 只跑快乐路径会让并发和安全问题漏到生产。
- 缓存失效不应影响正确性。
- 发布版本应和 changelog、tag、构建产物一致。

## 练习题

1. 为 `go-lab` 设计最小 CI 工作流。
2. 增加多操作系统测试矩阵。
3. 写一份 v0.1.0 发布 checklist。

## 官方参考链接

- [Command go](https://pkg.go.dev/cmd/go)
- [Go Modules Reference](https://go.dev/ref/mod)
- [Go Vulnerability Management](https://go.dev/doc/security/vuln/)

