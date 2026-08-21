# Video Trail

English | [中文](./README.zh-CN.md)

`video_trail.tox` is a lightweight TouchDesigner component that adds a fading motion trail to any image or video TOP. Bright areas persist and decay smoothly over time — a classic light-trail / motion-echo effect.

![Video Trail preview](./preview.png)

*Left: original input. Right: trail effect with `Trail Decay` at 0.95.*

Author: `uinipan`

## Compatibility

Tested with TouchDesigner 2023.11280 on Windows.

## Package contents

- `video_trail.tox` — the TouchDesigner component
- `README.md` — English documentation
- `README.zh-CN.md` — 中文说明

## Installation and wiring

1. Drag `video_trail.tox` into a TouchDesigner project.
2. Connect any TOP (image, Movie File In, render output, camera) to its input.
3. Use the component output as the processed TOP.

```text
Image or video TOP -> video_trail -> TOP with trail effect
```

One TOP input, one TOP output. Output resolution follows the input.

## How it works

Each frame, an internal buffer is multiplied by `Trail Decay` and then combined with the new frame using a per-pixel maximum:

```text
trail = max(trail * Decay, current frame)
```

New bright content appears instantly; old content fades exponentially. This means bright-on-dark content (lights, sparks, silhouettes, white-on-black graphics) produces the clearest trails.

## Trail controls

| Parameter | Range | Function |
| --- | --- | --- |
| `Trail Decay` | 0–1 | How much of the trail survives each frame. Higher = longer trails. `0.9` fades 10% per frame; `1.0` never fades (permanent maximum hold); `0` disables the effect (passes the current frame through). |

The fade is per frame, so trail duration in seconds depends on the project frame rate: at 60 fps, `0.9` gives a visible trail of roughly a third of a second; `0.95` roughly two thirds of a second.

## Troubleshooting

| Problem | Cause / fix |
| --- | --- |
| No visible trail | Input is mostly dark or low-contrast. Trails are most visible on bright subjects against dark backgrounds. |
| Screen slowly fills to white | `Trail Decay` is at or near `1.0` — the buffer only grows. Lower it (e.g. `0.85`–`0.95`). |
| Trail length feels wrong | Adjust `Trail Decay`; remember the effect is frame-based, so changing the project fps changes the trail duration. |
| Frame rate drops | Processing runs per-pixel on the CPU (Script TOP + NumPy). Reduce input resolution for better performance. |

## Technical notes

- One TOP input, one TOP output; resolution follows the input.
- CPU-based processing (Script TOP with NumPy per-pixel maximum-decay) — no GPU shader involved.
- Trail length is frame-rate dependent.
- Requires NumPy (bundled with TouchDesigner).
