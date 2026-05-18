# Ebitengine 从入门到精通：官方 examples 驱动教材

> 版本基线：Ebitengine v2.9.x；建议 Go 1.24+。  
> 写作日期：2026-05-18。  
> 教学方式：每章围绕一个或多个官方 examples 展开，先运行、再拆解、再重写、最后做小练习。

## 本教材适合谁

你应该已经能写基本 Go 程序，例如 `package main`、函数、结构体、方法、切片、map、接口、错误处理。你不需要有游戏开发经验。

Ebitengine 的特点是 API 少、概念直接：窗口、屏幕、图片、输入、音频、shader、平台构建都可以用普通 Go 工程方式组织。它不是 Unity/Godot 那种编辑器驱动引擎，而是“代码驱动”的 2D 游戏库/引擎。

## 学习路线

| 阶段 | 目标 | 官方 examples |
|---|---|---|
| 入门 | 能创建窗口、理解 `Update/Draw/Layout`、绘制基本图形 | `rotate`, `shapes` |
| 基础 | 掌握图片、坐标、矩阵变换、动画、输入 | `animation`, `sprites`, `keyboard`, `gamepad`, `wheel` |
| 中级 | 掌握资源、字体、地图、摄像机、碰撞、UI 状态 | `tiles`, `camera`, `text`, `texti18n`, `ui`, `platformer` |
| 高级 | 掌握音频、shader、vector、离屏渲染、性能优化 | `audio`, `sinewave`, `shader`, `vector`, `masking`, `blend`, `sprites` |
| 实战 | 做一个可发布的小型 2D 游戏，并能构建 Web/桌面版本 | `flappy`, `blocks`, `2048`, `snake` |

## 文件结构

- `00_学习准备与官方资料.md`
- `01_第一程序与游戏主循环.md`
- `02_图片坐标与矩阵变换.md`
- `03_输入系统与交互.md`
- `04_动画精灵与场景组织.md`
- `05_地图摄像机与碰撞.md`
- `06_文字UI与调试界面.md`
- `07_音频系统.md`
- `08_Shader与Vector绘图.md`
- `09_性能优化与工程化.md`
- `10_发布到桌面Web移动与Steam.md`
- `11_综合项目_小型飞行躲避游戏.md`
- `12_官方examples索引与练习.md`

## 推荐学习方法

每章按四步走：

1. 运行官方 example。
2. 用本章解释拆解关键 API。
3. 重写一个更小的教学版程序。
4. 完成本章练习，并把代码提交到 Git。

不要只复制代码。Ebitengine 最重要的能力是：把“游戏状态”放进结构体，把“状态推进”放进 `Update`，把“画面表现”放进 `Draw`，把“屏幕逻辑尺寸”放进 `Layout`。

