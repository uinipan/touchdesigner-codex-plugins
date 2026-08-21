# Square Tracker

English | [中文](./README.zh-CN.md)

`square_tracker.tox` is a reusable TouchDesigner TOP component based on a Blob Track workflow. It converts contrast regions in an image or video into tracked square forms with feedback, displacement, title overlay, and independent color controls.

![Square Tracker preview](./preview.png)

Author: `uinipan`

## Compatibility

Tested with TouchDesigner 2023.11280 on Windows.

## Installation

1. Drag `square_tracker.tox` into a TouchDesigner project.
2. Connect an image, Movie File In TOP, camera TOP, or any other TOP to its input.
3. Use the component output as the processed TOP.
4. Adjust the `Tracking`, `Visual`, and `Motion` parameter pages.

```text
Image or video TOP -> square_tracker -> tracked-square TOP
```

The component has one TOP input and one TOP output. Output resolution follows the incoming TOP automatically.

## Controls

### Tracking

| Parameter | Function |
| --- | --- |
| `Threshold` | Luminance threshold used to identify tracked regions. |
| `Min Blob Size` | Rejects small noisy regions. |
| `Max Blob Size` | Rejects overly large regions. |
| `Max Move Distance` | Maximum frame-to-frame movement tolerated for an existing track. |
| `Revive Time` | Time during which a lost blob can regain its former ID. |

### Visual

| Parameter | Function |
| --- | --- |
| `Trail Opacity` | Controls persistence of the feedback trail. |
| `Track Color` | Color of the Blob Track overlay. |
| `Square Material Color` | Color of the rendered square forms. |
| `Title` | Text overlaid in the finished composition. |
| `Title Text Color` | Color of the title text. |

### Motion

| Parameter | Function |
| --- | --- |
| `Noise Warp` | Strength of the source-side displacement. |
| `Feedback Warp` | Strength of the feedback displacement. |
| `Grow / Shrink` | Controls expansion or contraction of the final image. |

## Quick start

1. Start with `Threshold` around `0.2`.
2. Raise `Min Blob Size` if the tracker reacts to noise.
3. Lower `Max Move Distance` for stricter matching, or increase it for faster subjects.
4. Use `Trail Opacity` with `Feedback Warp` to balance persistence and motion.
5. Set the three color controls independently to separate tracking marks, squares, and title.

## Troubleshooting

| Problem | Adjustment |
| --- | --- |
| Too many small tracked regions | Raise `Min Blob Size` or raise `Threshold`. |
| The subject is not tracked | Lower `Threshold` or increase `Max Blob Size`. |
| IDs change too quickly | Increase `Max Move Distance` and/or `Revive Time`. |
| Feedback becomes too chaotic | Lower `Feedback Warp` or `Trail Opacity`. |
| Output does not match the source size | Ensure the input TOP is connected to the component input; the output follows it automatically. |

## Technical notes

- One TOP input and one TOP output.
- Output resolution follows the input resolution.
- Uses TouchDesigner Blob Track, instanced geometry, render TOPs, and feedback processing.
- Internal network remains available for learning and customization.
