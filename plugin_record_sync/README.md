# Record Sync

English | [中文](./README.zh-CN.md)

`record_sync.tox` is a TouchDesigner recording component. It records any TOP video stream, with an optional CHOP audio track, to a `.mov` movie file, with automatic file naming, auto-rename protection, cue-at-start, and automatic stop at the end of the input.

Author: `uinipan`

## Compatibility

Tested with TouchDesigner 2023.11280 on Windows.

## Package contents

- `record_sync.tox` — the TouchDesigner component
- `README.md` — English documentation
- `README.zh-CN.md` — 中文说明

## Installation and wiring

1. Drag `record_sync.tox` into a TouchDesigner project.
2. Connect a video TOP (e.g. Movie File In, render output, camera) to the component input. An audio CHOP can be connected as well; video and audio are independent and either one works alone.
3. The recorded picture is the wired TOP. The recorded audio is taken from the wired CHOP through the internal Movie File Out `Audio CHOP` binding.
4. The component passes video through to its output, so it can sit inline in a network.

```text
Video TOP ──╮
            ├─► record_sync ──► video passthrough OUT
Audio CHOP ─╯        └─► writes .mov file on record
```

## Quick start

1. Set `Output Folder` (or leave the project-folder default).
2. Check the `Naming Format` preview to confirm the output filename rule.
3. Pulse `Start Recording`. Pulse `Stop Recording` to end manually.
4. If the input has a detectable duration and `Auto Stop at Input End` is on, recording stops by itself when the input finishes.
5. Pulse `Open File Location` to open the folder with the recorded file selected.

## Record controls

| Parameter | Type | Function |
| --- | --- | --- |
| `Start Recording` | Button | Validates the output folder, detects the input duration, computes the output filename, cues the enabled sources, and starts writing on the next frame. |
| `Stop Recording` | Button | Stops writing and closes the movie file. Also cancels any pending auto-stop schedule. |
| `Auto Stop at Input End` | Toggle | When on and the input duration is detected, recording stops automatically after the input length. Has no effect for live sources (noise, camera, streams) — those always need a manual stop. |

## Inputs controls

| Parameter | Type | Function |
| --- | --- | --- |
| `Video Cue OP` | OP reference | The video source: what gets recorded, what gets cued, and whose duration is detected. Auto-filled when a TOP is wired in; can be set manually to an OP outside this component. |
| `Cue Video Input` | Toggle | When on, starting a recording restarts the video from its first frame (`cuepulse`). When off, recording captures from wherever the video currently is. |
| `Audio Cue OP` | OP reference | Same as `Video Cue OP`, for the audio CHOP source. |
| `Cue Audio Input` | Toggle | Same as `Cue Video Input`, for the audio source. |

Cue and auto-stop are independent: auto-stop depends only on `Auto Stop at Input End` plus a detectable input duration, whether or not cueing is enabled.

Note: the auto-stop timer measures the full input duration from the moment recording starts. If cueing is off and the video is already halfway through, auto-stop still fires after the full duration, not at the remaining playback time.

## Output controls

| Parameter | Type | Function |
| --- | --- | --- |
| `Output Folder` | Folder | Where recordings are written. Defaults to the current project folder, so it follows the `.toe` file. |
| `Choose Output Folder...` | Button | Opens a system folder picker and fills `Output Folder`. |
| `Auto Rename` | Toggle | When on and the target filename already exists, a numeric suffix is added (`_01`, `_02`, ...). When off, existing files are overwritten. |
| `Naming Format (Project + Input)` | Read-only | Live preview of the filename rule: `ProjectName_InputSource_NN.mov`. `NN` marks the auto-number slot; it does not mean that file exists yet. Updates automatically when the project name or input source changes. |

## Format controls

| Parameter | Type | Function |
| --- | --- | --- |
| `Video Codec` | Menu | Video encoder, e.g. `h264nvgpu` (NVIDIA GPU accelerated). |
| `Match Input FPS` | Toggle | When on and the input frame rate is detected, recording uses the input's real fps (e.g. 24). |
| `Frame Rate` | Number | Manual recording fps. Only used when `Match Input FPS` is off or the input fps cannot be detected. |
| `Quality` | 0–1 | Encoder quality. |
| `Audio Codec` | Menu | Audio encoder: `mp3`, `alac`, `pcm16`, `pcm24`, `pcm32`, `vorbis`. |

## Status area

| Parameter | Type | Function |
| --- | --- | --- |
| `Last Action` | Read-only | What happened most recently: which sources were cued, which file is being written, why recording stopped. The first place to check when something looks wrong. |
| `Open File Location` | Button | Opens the output folder in Explorer with the recorded file selected. Falls back to opening the folder if the file was deleted. |
| `Detected Input Duration` | Read-only | Detected input length, e.g. `124 frames @ 24.000 fps = 5.167 sec`. Shows `Not detected (live or unsupported TOP)` for live sources. |
| `Active Recording FPS` | Read-only | The fps actually used by the current/last recording. |
| `Recording Token` | Internal | Counter used by the auto-stop mechanism to ignore stale schedules. Do not modify. |

## File naming rule

```text
<ProjectName>_<InputSource>.mov          (first recording)
<ProjectName>_<InputSource>_01.mov       (Auto Rename suffixes)
```

Project and input names are sanitized: characters other than letters, numbers, `-`, `_` are replaced with `_` (including dots, so the filename never has more than one dot).

## Troubleshooting

| Problem | Cause / fix |
| --- | --- |
| Red flash `File extension must be .mov` | The output extension was changed to `.mp4`. With `h264nvgpu` in this TouchDesigner build, use the default `.mov` container. |
| Recording runs but no file appears | Check `Last Action` for the target path; make sure the video input is actually wired and cooking. |
| Auto-stop never fires | The input has no detectable duration (live/noise/stream). Stop manually, or use a Movie File In source. |
| No audio in the recording | Make sure an audio CHOP is wired (or `Audio Cue OP` points at one) and the audio CHOP is playing. |
| Recording frame rate looks wrong | Turn on `Match Input FPS` to follow the source, or set `Frame Rate` manually. |
| Auto-stop fires too late | It measures the full input duration from record start. Enable `Cue Video Input` so the input restarts together with the recording. |

## Technical notes

- Video input: TOP; audio input: CHOP (bound to the internal Movie File Out `Audio CHOP` parameter). Either can be used alone.
- Video passes through to the component output.
- Output container: `.mov`. Extension is part of the naming logic — do not rename outputs to `.mp4` when using NVIDIA H.264.
- Internal state (current file path) is kept in component storage; the panel is fully stateless.
- Cue OP references are auto-synchronized from wired inputs and can be overridden manually.
