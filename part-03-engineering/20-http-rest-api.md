# 20 HTTP 服务与 REST API

## 学习目标

- 使用 `net/http` 构建服务端。
- 设计 JSON 请求和响应。
- 将 `textlab` 暴露为 HTTP API。

## 工程场景

`go-lab` 从 CLI 扩展为服务：客户端提交文本，服务返回词频统计。核心业务不变，只新增传输层。

## 核心概念

`http.Handler` 是 Go Web 服务的核心接口。标准库足够构建清晰的小型 API。

```go
// server.go
package main

import (
	"encoding/json"
	"net/http"

	"example.com/go-lab/textlab"
)

type analyzeRequest struct {
	Text string `json:"text"`
}

func analyzeHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodPost {
		http.Error(w, "method not allowed", http.StatusMethodNotAllowed)
		return
	}

	var req analyzeRequest
	if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
		http.Error(w, "bad json", http.StatusBadRequest)
		return
	}

	report := textlab.CountWords(req.Text)
	w.Header().Set("Content-Type", "application/json")
	_ = json.NewEncoder(w).Encode(report)
}
```

## 渐进案例

启动服务：

```go
// main.go
package main

import (
	"log"
	"net/http"
)

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("POST /analyze", analyzeHandler)
	log.Fatal(http.ListenAndServe(":8080", mux))
}
```

## 常见坑

- 必须限制请求体大小，避免内存被大请求打爆。
- 不要把内部错误原样暴露给客户端。
- Handler 应尊重 `r.Context()` 的取消信号。

## 练习题

1. 增加 `GET /healthz`。
2. 使用 `http.MaxBytesReader` 限制请求体。
3. 使用 `httptest` 为 `POST /analyze` 写测试。

## 官方参考链接

- [Writing Web Applications](https://go.dev/doc/articles/wiki/)
- [Package net/http](https://pkg.go.dev/net/http)
- [Package net/http/httptest](https://pkg.go.dev/net/http/httptest)

