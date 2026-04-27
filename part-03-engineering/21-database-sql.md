# 21 database/sql 与事务

## 学习目标

- 理解 `database/sql` 是连接池和数据库抽象。
- 使用 context 执行查询和事务。
- 为 `go-lab` 保存分析历史。

## 工程场景

HTTP 服务需要记录每次分析任务：输入摘要、词数、创建时间和状态。数据库访问应和业务逻辑隔离。

## 核心概念

`sql.DB` 不是单个连接，而是并发安全的连接池。事务用 `sql.Tx` 表达，必须明确 `Commit` 或 `Rollback`。

```go
// store.go
package main

import (
	"context"
	"database/sql"
	"time"
)

type Store struct {
	DB *sql.DB
}

func (s Store) SaveAnalysis(ctx context.Context, total int) error {
	_, err := s.DB.ExecContext(ctx,
		`INSERT INTO analyses(total_words, created_at) VALUES (?, ?)`,
		total,
		time.Now().UTC(),
	)
	return err
}
```

## 渐进案例

事务模板：

```go
// tx.go
package main

import (
	"context"
	"database/sql"
)

func withTx(ctx context.Context, db *sql.DB, fn func(*sql.Tx) error) error {
	tx, err := db.BeginTx(ctx, nil)
	if err != nil {
		return err
	}
	defer tx.Rollback()

	if err := fn(tx); err != nil {
		return err
	}
	return tx.Commit()
}
```

## 常见坑

- 每一行查询结果都要关闭 `Rows`。
- 不要拼接用户输入构造 SQL，使用参数。
- 连接池参数要按数据库和服务并发能力调优。

## 练习题

1. 设计 `analyses` 表结构。
2. 实现 `ListRecent(ctx, limit)`。
3. 为事务函数设计一个失败路径测试。

## 官方参考链接

- [Accessing relational databases](https://go.dev/doc/database/)
- [Executing transactions](https://go.dev/doc/database/execute-transactions)
- [Avoiding SQL injection risk](https://go.dev/doc/database/sql-injection)

