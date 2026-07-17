# Audio Reactive Tools

English | [中文](./README.zh-CN.md)

`audio_reactive_tools.tox` is a reusable TouchDesigner CHOP component that turns one audio input into four practical control outputs: amplitude envelope, accumulated speed, kick/snare triggers, and kick/snare counts.

It is intended for projects that need stable audio control signals without rebuilding the same Audio Analysis, Speed, Count, and output Null network each time.

Author: `uinipan`

## Installation

1. Drag `audio_reactive_tools.tox` into a TouchDesigner project.
2. Connect an Audio File In CHOP, Audio Device In CHOP, or another audio CHOP to input 0.
3. Connect the four outputs to visual parameters or downstream CHOP processing.

```text
Audio CHOP -> audio_reactive_tools

out_envelope -> continuous visual intensity
out_speed    -> accumulated motion
out_triggers -> kick/snare event pulses
out_count    -> kick/snare event counts
```

## Outputs

| Order | Output | Signal | Typical uses |
| ---: | --- | --- | --- |
| 0 | `out_envelope` | Continuous amplitude envelope | Brightness, scale, feedback gain, displacement |
| 1 | `out_speed` | Accumulated value driven by audio energy | Noise phase, scrolling, rotation, time offset |
| 2 | `out_triggers` | Kick/snare pulses | Flash, switch, burst, reset |
| 3 | `out_count` | Accumulated kick/snare counts | Switch index, state cycling, event totals |

## Envelope controls

| Parameter | Function |
| --- | --- |
| `Env Input Gain` | Raises or lowers audio before envelope analysis. |
| `Env Smooth Width` | Controls envelope smoothing; higher is steadier, lower is more responsive. |
| `Env Output Gain` | Multiplies the final envelope signal. |
| `Env Output Offset` | Adds a baseline value to the envelope output. |
| `Env Reset Speed` | Resets the accumulated `out_speed` value. |

## Beat and count controls

| Parameter | Function |
| --- | --- |
| `Kick Active` | Enables kick detection. |
| `Kick Threshold` | Sets how strict kick triggering is. |
| `Snare Active` | Enables snare detection. |
| `Snare Threshold` | Sets how strict snare triggering is. |
| `Count Max` | Sets the maximum kick/snare count. |
| `Reset Count` | Resets `out_count`. |
| `Beat Sensitivity` | Amplifies energy before kick/snare detection. |

## Choosing the right output

- Use `out_envelope` for continuous loudness-driven changes.
- Use `out_speed` when audio energy should keep motion advancing.
- Use `out_triggers` for one-frame or short event actions.
- Use `out_count` to rotate through states or keep an event total.

Kick usually represents low-frequency impact. Snare usually represents a sharper mid/high-frequency transient. Both are event signals, not continuous loudness values.

## Threshold tuning

- Too few triggers: lower the matching threshold or raise `Beat Sensitivity`.
- Too many false triggers: raise the matching threshold or lower `Beat Sensitivity`.
- Tune kick and snare thresholds independently after sensitivity is roughly correct.

## Path examples

```python
op('/project1/audio_reactive_tools/out_envelope')
op('/project1/audio_reactive_tools/out_speed')
op('/project1/audio_reactive_tools/out_triggers')
op('/project1/audio_reactive_tools/out_count')
```

## Scope

The component outputs control data only. It does not include a full spectrum-texture lane. Build a separate spectrum branch when a project needs CHOP-to-TOP frequency textures, reduced frequency bands, or shader spectrum input.

