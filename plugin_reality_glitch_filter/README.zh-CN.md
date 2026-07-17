# Reality Glitch Filter 现实故障滤镜

[English](./README.md) | 中文

`reality_glitch_filter.tox` 是一个实时 TouchDesigner GLSL TOP 故障滤镜，把 RGB 分离、扫描线、横向抖动、图块错位、模拟噪点、色阶压缩和调色整合在同一个可复用组件中。

![Reality Glitch Filter 预览](./preview.png)

作者：`uinipan`

## 兼容性

已在 Windows、TouchDesigner 2023.11280 中测试。

## 安装与连接

1. 将 `reality_glitch_filter.tox` 拖入 TouchDesigner 工程。
2. 把图片、Movie File In TOP 或其他彩色 TOP 接到插件输入。
3. 从插件输出获取处理后的 TOP。
4. 选择预设，点击 `Apply Preset`，再细调参数。

```text
图片或视频 TOP -> reality_glitch_filter -> 故障 TOP 输出
```

插件为单 TOP 输入、单 TOP 输出，分辨率自动跟随输入 TOP。

## 预设说明

| 预设 | 视觉特点 |
| --- | --- |
| `Cyberpunk` | 均衡组合 RGB 分离、扫描线、抖动、图块和青色调。 |
| `Analog Noise` | 强化噪点、扫描线和模拟信号质感。 |
| `RGB Split` | 以色彩通道错位为主，其他扰动较轻。 |
| `Scanline Jitter` | 密集扫描线和明显的横向行抖动。 |
| `Tile Jitter` | 强烈图块错位和碎片运动。 |
| `Datamosh Lite` | 类似压缩损坏的块状运动，带色阶压缩和绿色调。 |

只选择预设名称不会立即改动参数，选择后需要点击一次 `Apply Preset`。

## 参数说明

| 参数 | 功能 |
| --- | --- |
| `Amount` | 整体故障效果强度。 |
| `Speed` | 抖动、噪点、图块和扫描线的动画速度。 |
| `RGB Split` | 红、绿、蓝采样之间的水平偏移距离。 |
| `Scanlines` | 动态横向扫描线的强度。 |
| `Jitter` | 横向行错位的出现密度和位移距离。 |
| `Blocks` | 图块/瓦片错位强度。 |
| `Noise` | 细颗粒模拟噪点和云状噪声强度。 |
| `Posterize` | 减少颜色层级，产生色阶压缩。 |
| `Brightness` | 整体增加或降低亮度。 |
| `Contrast` | 以中灰为中心拉伸或压缩对比度。 |
| `Tint` | 用 RGB 三个数值进行整体乘色调色。 |

大部分参数的面板常用范围为 0–1，但没有强制 Clamp。部分内置预设会使用大于 1 的 `Speed`、`Contrast`，或小于 0 的 `Brightness`。

## 调试视图

`Debug Mode` 用于查看着色器中间数据：

- `Final`：最终效果；
- `UV`：错位后的采样坐标；
- `Noise`：动态程序噪声；
- `Mask`：行、图块和瓦片激活遮罩。

正常输出时应切回 `Final`。

## 快速效果配方

### 干净的 RGB 错位

- 从 `RGB Split` 预设开始；
- 降低 `Noise`、`Blocks` 和 `Scanlines`；
- 调节 `RGB Split` 和 `Amount`。

### 模拟显示器

- 从 `Analog Noise` 开始；
- 提高 `Scanlines` 和 `Noise`；
- 保持轻微 `RGB Split`。

### 强烈数字破碎

- 从 `Tile Jitter` 或 `Datamosh Lite` 开始；
- 提高 `Blocks`、`Jitter` 和 `Posterize`；
- 使用 `Speed` 控制运动强度。

## 常见问题

| 问题 | 调整方法 |
| --- | --- |
| 效果过于混乱 | 降低 `Amount`、`Jitter` 和 `Blocks`。 |
| 颜色错位距离太大 | 降低 `RGB Split`。 |
| 画面噪点太多 | 降低 `Noise` 和 `Scanlines`。 |
| 动画太快 | 降低 `Speed`。 |
| 画面太暗或太亮 | 先把 `Brightness` 归零，再调整 `Contrast`。 |
| 输出显示 UV、噪声或遮罩 | 把 `Debug Mode` 切回 `Final`。 |

## 技术信息

- 单 TOP 输入、单 TOP 输出；
- 基于 GLSL TOP，分辨率跟随输入；
- 使用程序 Hash/Noise，不依赖额外纹理；
- 内置预设表和调试视图；
- GLSL 编译成功，全部内部节点无错误或警告。

## 灵感来源与实现差异

效果类别参考了 [XanderXu/RealityGlitchArt](https://github.com/XanderXu/RealityGlitchArt)。原项目是 Reality Composer Pro / visionOS 的 Shader Graph 示例；本插件是面向 TouchDesigner 2D TOP 图像处理的独立 GLSL 重写，不依赖 RealityKit、Reality Composer Pro 或 Apple 平台。

