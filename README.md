# Go 从入门到精通：工程化与 AI 时代实践

本仓库是一套中文 Go 教材，面向希望从零基础进入 Go 工程开发，并进一步掌握高并发、工具链、服务端工程、AI Agent 与 MCP 的读者。教材以 Go 1.26.x 为基线：Go 1.26.0 于 2026-02-10 发布，Go 1.26.2 于 2026-04-07 发布。阅读时请优先安装当前稳定维护版。

教材不是官方文档翻译，而是以一个贯穿全书的 `go-lab` 工程为主线，把语言特性放进真实工程场景中学习。所有代码以内嵌 Markdown 代码块交付，不额外创建源码目录。

## 学习路径

1. 入门阶段：写出能运行的 Go 程序，理解变量、函数、集合、结构体、方法和错误。
2. 惯用阶段：掌握接口、泛型、包设计、测试、文档和可维护代码风格。
3. 工程阶段：使用模块、工作区、工具链、配置、日志、数据库和 HTTP API 构建服务。
4. 并发性能阶段：掌握 goroutine、channel、context、worker pool、限流、竞态检测、pprof 和 GC。
5. 生产安全阶段：理解部署、CI、漏洞管理、供应链、兼容性、可观测性和故障排查。
6. AI/MCP 阶段：以 provider-neutral 架构实现 Agent、工具调用、MCP Server/Client 和权限审计。

## 章节索引

### Part 01: Foundations

- [01 环境、模块与第一个程序](part-01-foundations/01-hello-go.md)
- [02 值、变量与类型](part-01-foundations/02-values-variables-types.md)
- [03 控制流与函数](part-01-foundations/03-control-flow-functions.md)
- [04 数组、切片与映射](part-01-foundations/04-slices-maps.md)
- [05 结构体、方法与组合](part-01-foundations/05-structs-methods-composition.md)
- [06 错误处理入门](part-01-foundations/06-errors-basics.md)
- [07 命令行文本工具 go-lab](part-01-foundations/07-cli-text-tool.md)

### Part 02: Idiomatic Go

- [08 包、可见性与项目边界](part-02-idiomatic-go/08-packages-visibility.md)
- [09 接口与依赖反转](part-02-idiomatic-go/09-interfaces-design.md)
- [10 泛型与约束](part-02-idiomatic-go/10-generics.md)
- [11 错误链、哨兵错误与自定义错误](part-02-idiomatic-go/11-errors-advanced.md)
- [12 测试、表驱动与基准测试](part-02-idiomatic-go/12-testing-benchmark.md)
- [13 文档、示例与代码风格](part-02-idiomatic-go/13-docs-style.md)
- [14 重构 go-lab 为可测试库](part-02-idiomatic-go/14-refactor-go-lab.md)

### Part 03: Engineering

- [15 Go Modules 与依赖管理](part-03-engineering/15-modules-dependencies.md)
- [16 Workspaces 与多模块开发](part-03-engineering/16-workspaces.md)
- [17 go 命令、vet、fmt 与工具链](part-03-engineering/17-toolchain.md)
- [18 配置、环境变量与命令行参数](part-03-engineering/18-configuration.md)
- [19 结构化日志与 slog](part-03-engineering/19-logging-slog.md)
- [20 HTTP 服务与 REST API](part-03-engineering/20-http-rest-api.md)
- [21 database/sql 与事务](part-03-engineering/21-database-sql.md)
- [22 Fuzzing、Coverage 与质量门禁](part-03-engineering/22-fuzzing-coverage.md)

### Part 04: Concurrency & Performance

- [23 Goroutine 与调度直觉](part-04-concurrency-performance/23-goroutines-scheduler.md)
- [24 Channel、关闭语义与 select](part-04-concurrency-performance/24-channels-select.md)
- [25 context、取消与超时](part-04-concurrency-performance/25-context-cancellation.md)
- [26 Worker Pool、限流与背压](part-04-concurrency-performance/26-worker-pool-rate-limit.md)
- [27 sync、atomic 与竞态检测](part-04-concurrency-performance/27-sync-atomic-race.md)
- [28 pprof、trace 与性能分析](part-04-concurrency-performance/28-pprof-trace.md)
- [29 GC、内存模型与容量规划](part-04-concurrency-performance/29-gc-memory-model.md)

### Part 05: Production & Security

- [30 构建、交付与部署](part-05-production-security/30-build-deploy.md)
- [31 CI、测试矩阵与发布流程](part-05-production-security/31-ci-release.md)
- [32 漏洞管理与 govulncheck](part-05-production-security/32-vulnerability-management.md)
- [33 可观测性与生产诊断](part-05-production-security/33-observability-diagnostics.md)
- [34 兼容性、版本化与长期维护](part-05-production-security/34-compatibility-maintenance.md)

### Part 06: AI Agent & MCP

- [35 Agent 架构与 Provider 抽象](part-06-ai-agent-mcp/35-agent-provider-abstraction.md)
- [36 工具调用与安全执行](part-06-ai-agent-mcp/36-tool-calling.md)
- [37 记忆、上下文与检索](part-06-ai-agent-mcp/37-memory-context-retrieval.md)
- [38 MCP 协议与 JSON-RPC](part-06-ai-agent-mcp/38-mcp-protocol-jsonrpc.md)
- [39 构建 MCP Server](part-06-ai-agent-mcp/39-mcp-server.md)
- [40 MCP Client、权限与审计](part-06-ai-agent-mcp/40-mcp-client-security.md)

### Appendix

- [命令速查](appendix/commands-cheatsheet.md)
- [标准库索引](appendix/standard-library-map.md)
- [练习提示与验收标准](appendix/exercise-hints.md)
- [官方参考资料](appendix/references.md)

## 官方资料基线

- [Go Documentation](https://go.dev/doc/)
- [The Go Programming Language Specification](https://go.dev/ref/spec)
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go Release History](https://go.dev/doc/devel/release)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Modules Reference](https://go.dev/ref/mod)
- [Go Diagnostics](https://go.dev/doc/diagnostics)
- [Go Vulnerability Management](https://go.dev/doc/security/vuln/)
- [Model Context Protocol Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)

