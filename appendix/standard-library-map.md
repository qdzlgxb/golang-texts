# 标准库索引

## 入门常用

- `fmt`：格式化输入输出。
- `strings`：字符串查找、切分、替换。
- `strconv`：字符串和数字转换。
- `errors`：创建和判断错误。
- `flag`：命令行参数解析。
- `os`：文件、环境变量、进程退出。
- `io` / `bufio`：流式读写和缓冲扫描。

## 工程服务

- `net/http`：HTTP 客户端和服务端。
- `encoding/json`：JSON 编解码。
- `database/sql`：关系数据库抽象。
- `context`：请求范围取消、超时和值传递。
- `log/slog`：结构化日志。
- `crypto/tls` / `crypto/x509`：TLS 与证书。

## 并发性能

- `sync`：互斥锁、读写锁、等待组、Once、Pool。
- `sync/atomic`：原子操作。
- `runtime`：运行时信息、调度和 GC 控制。
- `runtime/pprof` / `net/http/pprof`：性能剖析。
- `runtime/trace`：运行时事件追踪。
- `testing`：测试、基准测试、fuzzing。

## 编译与工具

- `go/ast`、`go/parser`、`go/token`：语法树与源码分析。
- `go/types`：类型检查。
- `debug/buildinfo`：读取 Go 二进制构建信息。

## 官方参考

- [Standard library](https://pkg.go.dev/std)
- [Package documentation](https://pkg.go.dev/)

