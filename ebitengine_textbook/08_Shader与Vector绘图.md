# 08. Shader 与 Vector 绘图

对应官方 examples：

```powershell
go run github.com/hajimehoshi/ebiten/v2/examples/shader@latest
go run github.com/hajimehoshi/ebiten/v2/examples/vector@latest
go run github.com/hajimehoshi/ebiten/v2/examples/masking@latest
go run github.com/hajimehoshi/ebiten/v2/examples/blur@latest
go run github.com/hajimehoshi/ebiten/v2/examples/blend@latest
```

## 8.1 什么时候需要 Shader

普通绘制解决不了或效率不够时，考虑 shader：

- 水波、扭曲、发光、溶解。
- 屏幕后处理。
- 大量像素级计算。
- 自定义采样和颜色变换。

Ebitengine 的 shader 语言叫 **Kage**。它语法接近 Go，但不是完整 Go。它主要写 fragment shader，也就是“每个像素如何计算颜色”。

## 8.2 最小 Kage Shader

```go
var shaderSrc = []byte(`
package main

//kage:unit pixels

var Time float

func Fragment(dstPos vec4, srcPos vec2, color vec4) vec4 {
    v := 0.5 + 0.5*sin(Time + dstPos.x*0.04)
    return vec4(v, 0.3, 1.0-v, 1.0)
}
`)
```

编译：

```go
shader, err := ebiten.NewShader(shaderSrc)
if err != nil {
    return nil, err
}
```

绘制：

```go
op := &ebiten.DrawRectShaderOptions{}
op.Uniforms = map[string]any{
    "Time": float32(g.tick) / 60,
}
screen.DrawRectShader(screenW, screenH, g.shader, op)
```

## 8.3 Shader 程序结构

常见签名：

```go
func Fragment(dstPos vec4, srcPos vec2, color vec4) vec4
```

参数含义：

- `dstPos`：目标像素位置。
- `srcPos`：源图采样位置。
- `color`：顶点或绘制选项传入的颜色信息。
- 返回值：当前像素颜色，RGBA 分量通常是 0..1。

Uniform：

```go
var Time float
var Strength float
```

Go 侧传：

```go
op.Uniforms = map[string]any{
    "Time": float32(t),
    "Strength": float32(0.8),
}
```

## 8.4 Shader 调试经验

- 先让 shader 返回纯色，确认它被执行。
- 再显示坐标渐变，确认坐标单位。
- 最后加时间、纹理采样、复杂数学。
- Kage 不是完整 Go，不要使用 Go 的复杂类型、goroutine、slice、import。

## 8.5 Vector 绘图

Ebitengine v2.9 对 vector 渲染做了重要增强。常用 API：

```go
vector.FillRect(screen, x, y, w, h, clr, true)
vector.FillCircle(screen, cx, cy, r, clr, true)
```

路径：

```go
var p vector.Path
p.MoveTo(10, 10)
p.LineTo(100, 40)
p.LineTo(40, 100)
p.Close()

vector.FillPath(screen, &p, color.RGBA{255, 100, 80, 255}, nil)
```

Vector 适合：

- UI 形状。
- 调试可视化。
- 程序生成图形。
- 不依赖位图资源的小型效果。

## 8.6 Masking 与 Blend

Masking 的思想：先构造遮罩，再只显示遮罩允许的区域。Blend 的思想：控制源颜色和目标颜色如何混合。

常见用途：

- 圆形视野。
- 黑暗中的手电筒。
- UI 裁剪区域。
- 粒子加法混合。
- 受击闪白、溶解特效。

## 8.7 离屏渲染后处理

典型流程：

1. 世界画到 `worldImage`。
2. UI 画到 `uiImage` 或直接画到屏幕。
3. 用 shader 把 `worldImage` 画回屏幕。
4. 最后画 UI。

```go
g.world.Clear()
g.drawWorld(g.world)

op := &ebiten.DrawRectShaderOptions{}
op.Images[0] = g.world
screen.DrawRectShader(screenW, screenH, g.postShader, op)

g.drawUI(screen)
```

## 8.8 本章练习

1. 写一个全屏渐变 shader。
2. 给玩家加受击闪烁 shader。
3. 用 vector 绘制一个血条。
4. 做一个圆形遮罩，只显示玩家周围区域。

