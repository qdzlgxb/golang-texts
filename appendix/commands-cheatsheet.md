# 命令速查

## 模块与依赖

```sh
go mod init example.com/go-lab
go mod tidy
go get example.com/some/module@latest
go list -m all
go env GOPATH GOMOD GOWORK
```

## 构建与运行

```sh
go run .
go build ./...
go install ./cmd/go-lab
go version -m ./go-lab
```

## 测试与质量

```sh
go test ./...
go test -race ./...
go test -cover ./...
go test -bench=. ./...
go test -fuzz=Fuzz -fuzztime=30s ./...
go vet ./...
govulncheck ./...
```

## 性能与诊断

```sh
go test -bench=. -benchmem ./...
go test -run=^$ -bench=. -cpuprofile cpu.out ./...
go tool pprof cpu.out
go test -trace trace.out ./...
go tool trace trace.out
```

## 工作区

```sh
go work init ./app ./lib
go work use ./tools
go work sync
```

## 官方参考

- [Command go](https://pkg.go.dev/cmd/go)
- [Go Modules Reference](https://go.dev/ref/mod)
- [Diagnostics](https://go.dev/doc/diagnostics)

