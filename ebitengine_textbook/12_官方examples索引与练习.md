# 12. 官方 examples 索引与练习

本章把官方 examples 转成学习任务。建议按顺序运行，每个 example 不只是“看效果”，而是回答：它解决了什么问题？用了哪些 API？我能不能写一个更小版本？

## 12.1 图形基础

| Example | 学习目标 | 练习 |
|---|---|---|
| `animation` | 帧动画 | 用 4 个矩形区域做行走动画 |
| `rotate` | `GeoM` 平移/旋转 | 改成围绕鼠标旋转 |
| `sprites` | 大量精灵与批处理 | 让精灵数量可调 |
| `tiles` | Tile map | 做一个 20×15 地图 |
| `stars` | 简单粒子/星空 | 加速度和远近层次 |
| `particles` | 粒子系统 | 爆炸粒子，生命周期淡出 |

## 12.2 图形高级

| Example | 学习目标 | 练习 |
|---|---|---|
| `blend` | 混合模式 | 做发光粒子 |
| `masking` | 遮罩 | 做圆形视野 |
| `shader` | Kage shader | 做时间变化渐变 |
| `vector` | 矢量绘制 | 做血条和技能冷却圈 |
| `raycasting` | 伪 3D/射线 | 做简化迷宫视野 |
| `perspective` | 透视变换 | 做卡片翻转效果 |
| `isometric` | 等距地图 | 做 5×5 菱形地图 |

## 12.3 输入

| Example | 学习目标 | 练习 |
|---|---|---|
| `keyboard` | 键盘状态 | 做键位调试器 |
| `gamepad` | 手柄输入 | 支持左摇杆移动 |
| `wheel` | 鼠标滚轮 | 缩放摄像机 |
| `textinput` | 文本输入 | 做名字输入框 |
| `touch` | 触摸 | 做移动端按钮 |
| `dropfile` | 拖拽文件 | 拖入图片并显示 |

## 12.4 音频

| Example | 学习目标 | 练习 |
|---|---|---|
| `audio` | 播放器综合案例 | 做 BGM 设置面板 |
| `wav` | 播放 WAV | 播放点击音效 |
| `sinewave` | 程序生成音频 | 做简单 beep |
| `piano` | 键盘触发声音 | 做小键盘钢琴 |

## 12.5 完整游戏

| Example | 学习目标 | 练习 |
|---|---|---|
| `flappy` | 完整小游戏循环 | 改成横向躲避游戏 |
| `blocks` | 方块类游戏 | 增加分数和关卡 |
| `2048` | 棋盘状态 | 增加撤销 |
| `snake` | 网格移动 | 增加障碍和加速 |
| `platformer` | 平台跳跃 | 增加二段跳 |

## 12.6 从 examples 到自己的游戏

不要直接把 example 代码粘进项目，而是抽取思想：

- `rotate` → `Transform`/`Sprite.Draw`。
- `keyboard` → `Input` 动作映射层。
- `sprites` → 批量对象更新与绘制。
- `tiles` → `World`/`TileMap`。
- `audio` → `AudioManager`。
- `shader` → `Effect`/`PostProcess`。
- `flappy` → Scene 与完整玩法闭环。

## 12.7 终极练习路线

1. 第一天：窗口、移动、绘制。
2. 第二天：输入映射、发射子弹、碰撞。
3. 第三天：场景系统、标题和结束。
4. 第四天：音效和粒子。
5. 第五天：资源 embed 和项目结构。
6. 第六天：shader 或 vector polish。
7. 第七天：WebAssembly 和 Windows 构建。

完成后，你已经具备使用 Ebitengine 独立制作小型 2D 游戏的能力。

