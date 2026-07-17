# Audio Reactive Tools 音频响应工具

[English](./README.md) | 中文

`audio_reactive_tools.tox` 是一个可复用的 TouchDesigner CHOP 插件，把一路音频输入转换成四组常用控制数据：整体音量包络、累计速度、kick/snare 触发和 kick/snare 计数。

适合需要稳定音频控制信号，又不想每次重新搭 Audio Analysis、Speed、Count 和输出 Null 的工程。

作者：`uinipan`

## 安装与连接

1. 将 `audio_reactive_tools.tox` 拖入 TouchDesigner 工程。
2. 把 Audio File In CHOP、Audio Device In CHOP 或其他音频 CHOP 接到输入 0。
3. 将四个输出连接到视觉参数或后续 CHOP 处理。

```text
音频 CHOP -> audio_reactive_tools

out_envelope -> 连续视觉强度
out_speed    -> 累计运动推进
out_triggers -> kick/snare 事件脉冲
out_count    -> kick/snare 累计计数
```

## 输出说明

| 顺序 | 输出 | 信号 | 常见用途 |
| ---: | --- | --- | --- |
| 0 | `out_envelope` | 连续音量包络 | 亮度、缩放、feedback gain、位移 |
| 1 | `out_speed` | 音频能量推动的累计值 | noise phase、滚动、旋转、time offset |
| 2 | `out_triggers` | kick/snare 脉冲 | 闪白、切换、burst、reset |
| 3 | `out_count` | kick/snare 累计次数 | Switch index、状态轮换、事件总数 |

## Envelope 参数

| 参数 | 功能 |
| --- | --- |
| `Env Input Gain` | 调整进入包络分析前的音频增益。 |
| `Env Smooth Width` | 控制包络平滑；越高越稳定，越低越跟手。 |
| `Env Output Gain` | 放大最终包络输出。 |
| `Env Output Offset` | 给包络输出增加基础偏移。 |
| `Env Reset Speed` | 重置累计的 `out_speed`。 |

## Beat Count 参数

| 参数 | 功能 |
| --- | --- |
| `Kick Active` | 启用或关闭 kick 检测。 |
| `Kick Threshold` | 控制 kick 触发严格程度。 |
| `Snare Active` | 启用或关闭 snare 检测。 |
| `Snare Threshold` | 控制 snare 触发严格程度。 |
| `Count Max` | 设置 kick/snare 计数最大值。 |
| `Reset Count` | 重置 `out_count`。 |
| `Beat Sensitivity` | 在 kick/snare 检测前放大能量。 |

## 如何选择输出

- 持续跟随音量强弱：使用 `out_envelope`；
- 让音频持续推动运动：使用 `out_speed`；
- 每次打点执行一个动作：使用 `out_triggers`；
- 轮换状态或累计次数：使用 `out_count`。

kick 通常代表低频冲击，snare 通常代表更清脆的中高频瞬态。两者都是事件信号，不是连续音量值。

## Threshold 调节

- 触发太少：降低对应 Threshold，或提高 `Beat Sensitivity`；
- 误触发太多：提高对应 Threshold，或降低 `Beat Sensitivity`；
- 先把整体灵敏度调到大致正确，再分别微调 kick 和 snare。

## 路径示例

```python
op('/project1/audio_reactive_tools/out_envelope')
op('/project1/audio_reactive_tools/out_speed')
op('/project1/audio_reactive_tools/out_triggers')
op('/project1/audio_reactive_tools/out_count')
```

## 功能边界

插件只输出控制数据，不包含完整频谱纹理。如果工程需要 CHOP to TOP 频谱、频段 Reduce 或 Shader 频谱输入，应单独搭建 spectrum 分支。

