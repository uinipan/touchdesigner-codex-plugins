# Record Sync 同步录制器

[English](./README.md) | 中文

## 插件功能

`record_sync.tox` 是一个 TouchDesigner 录制组件，把任意 TOP 视频流（可带一路 CHOP 音频）录制成 `.mov` 文件。内置自动命名、自动编号防覆盖、开头 cue、输入播完自动停止等功能。

作者：`uinipan`

已在 TouchDesigner 2023.11280 / Windows 中测试。

## 包内容

- `record_sync.tox` — TouchDesigner 组件
- `README.md` — 英文文档
- `README.zh-CN.md` — 中文说明

## 安装与连接

1. 将 `record_sync.tox` 拖入 TouchDesigner 工程。
2. 把视频 TOP（Movie File In、渲染输出、摄像头等）接到组件输入；音频 CHOP 也可以一起接。视频和音频相互独立，只接一个也能工作。
3. 录制的画面来自接入的 TOP；音频通过内部 Movie File Out 的 `Audio CHOP` 绑定自动取自接入的 CHOP。
4. 组件会把视频原样透传到输出，可以串在网络中间使用。

```text
视频 TOP ──╮
           ├─► record_sync ──► 视频透传输出
音频 CHOP ─╯        └─► 录制时写出 .mov 文件
```

## 快速上手

1. 设置 `Output Folder`（输出文件夹），或用默认的工程目录。
2. 看一眼 `Naming Format` 预览，确认文件名规则。
3. 点 `Start Recording` 开始；点 `Stop Recording` 手动停止。
4. 如果输入能检测到时长且 `Auto Stop at Input End` 开着，输入播完会自动停止。
5. 点 `Open File Location` 打开输出目录并自动选中录好的文件。

## Record 录制控制

| 参数 | 类型 | 功能 |
| --- | --- | --- |
| `Start Recording` | 按钮 | 校验输出目录 → 检测输入时长 → 生成文件名 → 触发 cue（如果开启）→ 下一帧开始写入。 |
| `Stop Recording` | 按钮 | 停止写入并关闭文件，同时取消已排定的自动停止计划。 |
| `Auto Stop at Input End` | 开关 | 开 = 检测到输入时长时，播完自动停止。对直播、noise、摄像头等无时长输入无效，仍需手动停止。 |

## Inputs 输入源

| 参数 | 类型 | 功能 |
| --- | --- | --- |
| `Video Cue OP` | OP 引用 | 视频源：决定录谁、cue 谁、测谁的时长。插入 TOP 时自动填充，也可以手动指向插件外部的 OP。 |
| `Cue Video Input` | 开关 | 开 = 开始录制时视频跳回第 0 帧从头播放（`cuepulse`）；关 = 从视频当前位置开始录。 |
| `Audio Cue OP` | OP 引用 | 同上，针对音频 CHOP 源。 |
| `Cue Audio Input` | 开关 | 同上，针对音频源。 |

Cue 和自动停止互相独立：自动停止只看 `Auto Stop at Input End` 开关和输入时长，跟 cue 开不开无关。

注意：自动停止计时是从开始录制起算的"完整输入时长"。如果 cue 关着、视频已经播到一半，自动停止仍按完整时长触发，而不是按剩余播放时间。

## Output 输出

| 参数 | 类型 | 功能 |
| --- | --- | --- |
| `Output Folder` | 路径 | 录像保存位置，默认跟随当前工程目录（`.toe` 所在文件夹）。 |
| `Choose Output Folder...` | 按钮 | 弹出系统目录选择框，填入 `Output Folder`。 |
| `Auto Rename` | 开关 | 开 = 目标文件名已存在时自动加编号 `_01`、`_02`…；关 = 直接覆盖同名文件。 |
| `Naming Format (Project + Input)` | 只读 | 实时预览命名规则：`项目名_输入源_NN.mov`。`NN` 是编号占位符，不代表该文件已存在。项目名或输入源变化时自动更新。 |

## Format 格式

| 参数 | 类型 | 功能 |
| --- | --- | --- |
| `Video Codec` | 菜单 | 视频编码器，如 `h264nvgpu`（NVIDIA 显卡加速）。 |
| `Match Input FPS` | 开关 | 开 = 能检测到输入帧率时，按输入的真实帧率录制（如 24）。 |
| `Frame Rate` | 数值 | 手动录制帧率。仅在 `Match Input FPS` 关闭或检测不到输入帧率时生效。 |
| `Quality` | 0–1 | 编码画质。 |
| `Audio Codec` | 菜单 | 音频编码器：`mp3`、`alac`、`pcm16`、`pcm24`、`pcm32`、`vorbis`。 |

## Status 状态区

| 参数 | 类型 | 功能 |
| --- | --- | --- |
| `Last Action` | 只读 | 最近发生的操作：cue 了谁、正在写哪个文件、停止原因。出问题时先看这里。 |
| `Open File Location` | 按钮 | 打开输出目录并在资源管理器中选中录好的文件；文件已被删除时退化为只打开目录。 |
| `Detected Input Duration` | 只读 | 检测到的输入时长，如 `124帧 @ 24fps = 5.167秒`；直播类输入显示 `Not detected`。 |
| `Active Recording FPS` | 只读 | 本次/上次录制实际使用的帧率。 |
| `Recording Token` | 内部参数 | 自动停止机制用来识别过期计划的计数器，请勿修改。 |

## 文件命名规则

```text
<项目名>_<输入源名>.mov          （首次录制）
<项目名>_<输入源名>_01.mov       （Auto Rename 自动编号）
```

项目名和输入源名会做清洗：字母、数字、`-`、`_` 以外的字符（包括点号）都替换为 `_`，保证文件名里只有一个点（扩展名）。

## 常见问题

| 问题 | 原因 / 解决 |
| --- | --- |
| 红色报错一闪 `File extension must be .mov` | 输出扩展名被改成了 `.mp4`。本 TD 版本里 `h264nvgpu` 请使用默认的 `.mov` 容器。 |
| 录制跑了但没有文件 | 看 `Last Action` 里的目标路径；确认视频输入确实接入了且在 cook。 |
| 自动停止不触发 | 输入没有可检测时长（直播/noise/流）。手动停止，或换用 Movie File In 源。 |
| 录出来的文件没有声音 | 确认音频 CHOP 已接入（或 `Audio Cue OP` 指向了它），且音频 CHOP 正在播放。 |
| 录制帧率不对 | 开 `Match Input FPS` 跟随源帧率，或手动设置 `Frame Rate`。 |
| 自动停止触发太晚 | 它按完整输入时长计时。开 `Cue Video Input` 让输入和录制同时从头开始。 |

## 技术信息

- 视频输入：TOP；音频输入：CHOP（绑定到内部 Movie File Out 的 `Audio CHOP` 参数）。两者可独立使用。
- 视频透传到组件输出。
- 输出容器为 `.mov`，扩展名写在命名逻辑里——用 NVIDIA H.264 时不要把输出改成 `.mp4`。
- 当前文件路径等内部状态保存在组件 storage 中，面板本身无状态。
- Cue OP 引用会随接线自动同步，也支持手动覆盖。
