# Uinipan TouchDesigner 插件合集

[English](./README.md) | 中文

一组可复用的 TouchDesigner `.tox` 组件，覆盖音频响应控制、GLSL 故障处理、交互遮罩和图片粒子渲染。

作者：`uinipan`

## 插件列表

| 插件 | 类型 | 功能 |
| --- | --- | --- |
| [Audio Reactive Tools](./plugin_audio_reactive_tools/) | CHOP | 将一路音频转换成 envelope、累计 speed、kick/snare trigger 和 count。 |
| [Glitch Line Filter](./plugin_glitch_line_filter/) | TOP / GLSL | 将图像转换成黑底横向断裂素描线。 |
| [Image Particles](./plugin_image_particles/) | TOP / SOP / Render | 将图片或视频转换成动态彩色粒子。 |
| [Mouse Reveal Feedback](./plugin_mouse_reveal_feedback/) | TOP / Feedback | 使用鼠标绘制永久或自动消退的揭示/擦除遮罩。 |
| [Reality Glitch Filter](./plugin_reality_glitch_filter/) | TOP / GLSL | 整合 RGB 分离、扫描线、抖动、图块错位、模拟噪点和色阶压缩。 |

每个插件文件夹包含：

- 打包好的 `.tox` 组件；
- 英文 `README.md`；
- 中文 `README.zh-CN.md`；
- 适用时附带预览图。

## 环境要求

- TouchDesigner 2023 或更新版本；
- 当前插件使用 TouchDesigner 2023.11280、Windows 完成说明和测试；
- GLSL 和粒子插件建议使用兼容的独立显卡。

## 安装方法

1. 下载或克隆本仓库；
2. 打开需要使用的插件文件夹；
3. 将其中的 `.tox` 拖入 TouchDesigner 工程；
4. 根据插件自己的 README 连接输入输出并调节参数。

## 仓库结构

```text
plugin_uinipan/
├── plugin_audio_reactive_tools/
├── plugin_glitch_line_filter/
├── plugin_image_particles/
├── plugin_mouse_reveal_feedback/
├── plugin_reality_glitch_filter/
├── README.md
└── README.zh-CN.md
```

## 说明

- 这些组件以打包工具的形式使用，也可以进入内部网络学习和修改；
- 每个插件的分辨率行为、参数和常见问题都记录在对应 README；
- `Reality Glitch Filter` README 已注明视觉概念灵感来源；

## 许可证

本仓库使用 [MIT License](./LICENSE) 完全开源，允许在遵守许可证条款的前提下商用。
