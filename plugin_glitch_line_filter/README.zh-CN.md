# Glitch Line Filter 横向故障线滤镜

[English](./README.md) | 中文

## 插件功能

`glitch_line_filter.tox` 是一个 GLSL TOP 图像滤镜，用于把图片、视频或其他 TOP 转换成黑底、横向断裂白线组成的素描故障效果。

适合制作：

- 黑白横向扫描线；
- 断裂素描和技术制图质感；
- 复印机、传真和 Xerox 风格画面；
- 带轻微 RGB 错位的故障人像；
- 实时影像或 VJ 系统中的后期滤镜。

作者：`uinipan`

## 安装与连接

1. 将 `glitch_line_filter.tox` 拖入 TouchDesigner 工程。
2. 把图片、Movie File In TOP 或其他 TOP 接到插件左侧输入。
3. 从插件右侧获取处理后的 TOP 输出。
4. 打开 `Glitch Line`、`Portrait Lines` 和 `Bar Only` 参数页调节效果。

```text
图片或视频 TOP -> glitch_line_filter -> 故障线 TOP 输出
```

插件为单 TOP 输入、单 TOP 输出。输出分辨率跟随输入，像素格式为 8-bit RGBA。

## 快速调节

1. 在 `Preset` 中选择一个预设，然后点击 `Apply Preset`。
2. 用 `Amount` 调整整体故障效果强度。
3. 用 `Threshold` 和 `Fill Gain` 控制主体保留程度。
4. 用 `Line Density`、`Stroke Width`、`Dash Length` 和 `Dash Roughness`塑造横线。
5. 用 `Jitter`、`Blocks`、`Scan Break`、`Noise` 和 `Flicker` 增加动态故障。

## 预设说明

| 预设 | 视觉特点 |
| --- | --- |
| `White Line Glitch` | 均衡的黑底白色断线效果，适合作为通用起点。 |
| `Hard Scanline` | 更密、更硬的横向扫描线。 |
| `Broken Xerox` | 粗糙、破碎的复印机质感。 |
| `Fine Technical` | 更细、更干净的技术制图线条。 |
| `Ghost RGB` | 在白线基础上增加轻微 RGB 通道错位。 |

只选择预设名称不会立即改动参数，选择后需要点击一次 `Apply Preset`。

## Glitch Line 参数

| 参数 | 功能 |
| --- | --- |
| `Amount` | 整体效果强度。降低后更接近原始输入。 |
| `Edge Gain` | 强化检测到的边缘。数值过高会更像硬轮廓。 |
| `Threshold` | 控制哪些亮度区域进入线条遮罩。降低可保留更多主体。 |
| `Line Thickness` | 调节整体线条处理的厚度。 |
| `Glow` | 增加白色线条周围的亮度和扩散。 |
| `Jitter` | 增加不规则的横向抖动。 |
| `Blocks` | 增加块状破碎。 |
| `Scan Break` | 打断相邻扫描线区域的连续性。 |
| `Noise` | 增加细小随机扰动。 |
| `Flicker` | 调节随时间变化的亮度闪烁。 |
| `RGB Ghost` | 增加 RGB 通道分离和重影。 |
| `Invert Source` | 将输入逐渐混合到反相处理结果。 |

这些参数的面板常用映射范围为 0–1，但没有强制 Clamp；部分内置预设会使用大于 1 的 `Edge Gain` 或 `Line Thickness`。

## Portrait Lines 参数

| 参数 | 范围 | 功能 |
| --- | ---: | --- |
| `Fill Gain` | 常用 0–2+ | 提高主体内部的可见面积。只剩轮廓时可适当增加。 |
| `Line Density` | 0–1000 | 控制横线数量和行间距。数值越高，线条越密。 |
| `Dash Length` | 0–1 | 控制单段横线的长度。 |
| `Dash Roughness` | 0–1 | 增加横线断裂和粗糙程度。 |
| `Face Softness` | 0–1 | 软化主体遮罩，处理人像时尤其有用。 |
| `Stroke Width` | 0–1 | 控制每条横向笔画的粗细。 |

## Bar Only 参数

| 参数 | 范围 | 功能 |
| --- | ---: | --- |
| `Base Visibility` | 0–1 | 控制基础横线层的可见程度。 |

## 调试视图

`Debug Mode` 用于查看 GLSL 中间结果：

- `Final`：最终输出；
- `Edge`：边缘检测结果；
- `Mask`：主体和线条遮罩；
- `UV`：着色器使用的位移坐标。

正常输出时应切回 `Final`。

## 常见问题

| 问题 | 调整方法 |
| --- | --- |
| 主体几乎看不见 | 降低 `Threshold` 或提高 `Fill Gain`。 |
| 画面像生硬的恐怖轮廓 | 降低 `Edge Gain` 和 `Glow`，适当提高 `Face Softness`。 |
| 横线太密 | 降低 `Line Density`，或重新调整 `Stroke Width`。 |
| 故障效果过于混乱 | 降低 `Jitter`、`Blocks`、`Scan Break` 和 `Flicker`。 |
| 想要干净的黑白效果 | 从 `Fine Technical` 或 `White Line Glitch` 开始，并将 `RGB Ghost` 设为 0。 |
| 输出显示遮罩或 UV 图 | 把 `Debug Mode` 切回 `Final`。 |

## 技术信息

- 单 TOP 输入、单 TOP 输出；
- 基于 GLSL TOP；
- 输出分辨率跟随输入；
- 内置 5 个预设；
- 内置 Edge、Mask、UV 调试视图；
- 已在 TouchDesigner 2023.11280 中测试。
