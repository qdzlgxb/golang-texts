# 10. 发布到桌面、Web、移动与 Steam

对应官方文档和 examples：

```powershell
go run github.com/hajimehoshi/ebiten/v2/examples/blocks@latest
go run github.com/hajimehoshi/ebiten/v2/examples/flappy@latest
```

## 10.1 桌面发布

Windows：

```powershell
go build -o mygame.exe .
```

隐藏控制台：

```powershell
go build -o mygame.exe -ldflags="-H=windowsgui" .
```

交叉编译 Windows amd64：

```powershell
$Env:GOOS = 'windows'
$Env:GOARCH = 'amd64'
go build -o mygame_windows_amd64.exe .
Remove-Item Env:GOOS
Remove-Item Env:GOARCH
```

macOS/Linux 发布要考虑图标、签名、依赖、打包格式。Steam 上架还要处理审核、平台二进制、成就、语言、覆盖层等。

## 10.2 WebAssembly 快速预览

最简单方式：

```powershell
go run github.com/hajimehoshi/wasmserve@latest ./path/to/yourgame
```

然后访问：

```text
http://localhost:8080/
```

## 10.3 WebAssembly 手动构建

PowerShell：

```powershell
$Env:GOOS = 'js'
$Env:GOARCH = 'wasm'
go build -o mygame.wasm .
Remove-Item Env:GOOS
Remove-Item Env:GOARCH
```

复制 `wasm_exec.js`：

```powershell
$goroot = go env GOROOT
cp $goroot\lib\wasm\wasm_exec.js .
```

HTML：

```html
<!DOCTYPE html>
<script src="wasm_exec.js"></script>
<script>
const go = new Go();
WebAssembly.instantiateStreaming(fetch("mygame.wasm"), go.importObject).then(result => {
  go.run(result.instance);
});
</script>
```

## 10.4 Web 发布注意事项

- 资源大小会影响首屏加载。
- 音频可能需要用户手势后才能播放。
- 建议用 iframe 嵌入游戏页面。
- 如果有大量资源，可以考虑分离下载，而不是全部 embed 到 wasm。

## 10.5 移动端

Ebitengine 移动端通常不是生成完整 App，而是用 `ebitenmobile bind` 创建 Android/iOS 可用的库：

```powershell
go install github.com/hajimehoshi/ebiten/v2/cmd/ebitenmobile@latest
```

移动端入口不要调用 `ebiten.RunGame`，而是：

```go
func init() {
    mobile.SetGame(&yourgame.Game{})
}
```

Android 生成 `.aar`，iOS 生成 `.xcframework`，然后放入原生工程中。

## 10.6 Steam

Steam 发布不是简单 `go build`。你还需要：

- Steamworks SDK。
- 平台二进制。
- App ID。
- 覆盖层测试。
- 成就、云存档、语言等集成。
- Windows/macOS/Linux 不同平台包。

Windows 是最简单情况之一，因为 Ebitengine 在 Windows 上是 pure Go。Steam 文档还提到某些 Windows Steam 环境下 Go 应用冻结的 workaround，可在构建参数中设置相关 `ldflags`。

## 10.7 发布前检查表

- 窗口标题、图标、版本号。
- 音量设置持久化。
- 全屏/窗口化设置。
- 键位配置。
- 存档目录。
- 崩溃日志。
- Web 音频手势限制。
- 不同 DPI/窗口大小测试。
- 手柄插拔测试。

## 10.8 本章练习

1. 把你的小游戏构建成 Windows exe。
2. 用 wasmserve 在浏览器里运行。
3. 做一个 `--version` 或构建版本显示。
4. 写一份发布检查表，列出你的游戏还缺哪些项目。

