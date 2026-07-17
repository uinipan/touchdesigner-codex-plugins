# Mouse Reveal Feedback

English | [中文](./README.zh-CN.md)

`mouse_reveal_feedback.tox` is a TouchDesigner TOP component that uses a mouse-driven feedback mask to reveal or hide parts of an incoming image. Painted areas can remain permanently or fade over time, making the component useful for interactive reveals, trails, erasing effects, and live visual transitions.

![Mouse Reveal Feedback preview](./preview.png)

The preview above uses a pre-processed glitch image as its input. The component works with any TOP source.

Author: `uinipan`

## Compatibility

Tested with TouchDesigner 2023.11280 on Windows.

## Installation

1. Drag `mouse_reveal_feedback.tox` into a TouchDesigner project.
2. Connect an image, Movie File In TOP, or another TOP to the component input.
3. Move the mouse to paint into the feedback mask.
4. Use the component output as the revealed or erased image.
5. Adjust the `Reveal` parameter page.

```text
Image or video TOP -> mouse_reveal_feedback -> masked TOP output
```

The component has one TOP input and one TOP output. The mask and output resolutions follow the input TOP automatically.

## Controls

| Parameter | UI range | Function |
| --- | ---: | --- |
| `Reset Mask` | Pulse | Clears the accumulated feedback mask. |
| `Brush Size` | 0-0.1 | Controls the radius of the circular mouse brush. |
| `Brush Softness` | 0-0.2 | Softens the edge of each painted brush mark. |
| `Hold` | 0-1 | Controls how long painted areas remain. `1` preserves the mask; lower values fade it over time. |
| `Mask Softness` | 0-1 | Adjusts the overall softness/response curve of the accumulated mask. |
| `Mask Gain` | 0-1 | Multiplies mask strength before compositing. |
| `Reveal Mode` | On/Off | On shows painted areas; Off shows the inverse and hides painted areas. |

The numeric controls are not hard-clamped, although the listed ranges are the intended UI ranges.

## Common setups

### Permanent reveal

- Set `Reveal Mode` to On.
- Set `Hold` to 1.
- Paint with the mouse.
- Pulse `Reset Mask` to start again.

### Fading trail

- Set `Reveal Mode` to On.
- Lower `Hold` below 1.
- Lower values produce a faster fade.

### Mouse eraser

- Set `Reveal Mode` to Off.
- Paint with the mouse to hide the brushed areas.
- Use `Hold` to choose between permanent and fading erasure.

## Mask debugging

The component contains an internal `OUT_mask` TOP that displays the accumulated feedback mask.

![Accumulated feedback mask](./mask-preview.png)

Use it when the final output is unexpected:

- White areas are painted/revealed regions.
- Black areas are untouched regions.
- Soft gray edges come from `Brush Softness` and `Mask Softness`.

## Troubleshooting

| Problem | Adjustment |
| --- | --- |
| Painted areas disappear | Raise `Hold`; use 1 for permanent accumulation. |
| Trail never fades | Lower `Hold` below 1. |
| Brush is too small or large | Adjust `Brush Size`. |
| Edge is too hard | Raise `Brush Softness` or `Mask Softness`. |
| Effect is too weak | Raise `Mask Gain`. |
| Wrong side of the mask is visible | Toggle `Reveal Mode`. |
| Need to clear the drawing | Pulse `Reset Mask`. |

## Technical notes

- One TOP input and one TOP output.
- Input-driven resolution; no fixed-resolution dependency.
- Mouse In CHOP drives a Circle TOP brush.
- Feedback TOP stores the previous mask.
- Maximum compositing accumulates new brush marks.
- GLSL TOP combines the source and mask.
- Internal operators were checked with no errors or warnings.

