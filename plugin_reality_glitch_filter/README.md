# Reality Glitch Filter

English | [中文](./README.zh-CN.md)

`reality_glitch_filter.tox` is a real-time TouchDesigner GLSL TOP effect for cyberpunk-style image corruption. It combines RGB separation, per-channel RGB offset/scale adjustment, scanline distortion, horizontal jitter, block displacement, analog noise, posterization, and color grading in one reusable component.

![Reality Glitch Filter preview](./preview.png)

Author: `uinipan`

## Compatibility

Tested with TouchDesigner 2023.11280 on Windows.

## Installation

1. Drag `reality_glitch_filter.tox` into a TouchDesigner project.
2. Connect an image, Movie File In TOP, or another color TOP to its input.
3. Use the component output as the processed TOP.
4. Select a preset, pulse `Apply Preset`, then fine-tune the controls.

```text
Image or video TOP -> reality_glitch_filter -> glitch TOP output
```

The component has one TOP input and one TOP output. Its resolution follows the input TOP.

## Presets

| Preset | Character |
| --- | --- |
| `Cyberpunk` | Balanced RGB split, scanline, jitter, block, and cyan tint treatment. |
| `Analog_Noise` | Stronger grain, scanlines, and analog texture with a warm tint. |
| `RGB_Split` | Emphasizes chromatic channel separation with lighter distortion and a pink tint. |
| `Scanline_Jitter` | Dense scanlines and aggressive horizontal line displacement. |
| `Tile_Jitter` | Strong block/tile displacement and fragmented motion with a warm yellow tint. |
| `Datamosh_Lite` | Blocky compression-like motion with posterization and a green tint. |

Selecting a preset does not immediately change the parameters. Pulse `Apply Preset` after choosing it.

Applying a preset sets all main effect controls and the `Tint` channels, and resets the `RGB Adjust` page to neutral (offsets `0`, scales `1`). Per-channel adjustments made on the `RGB Adjust` page are replaced on each preset apply — fine-tune them after choosing a preset.

## Reality Glitch controls

| Parameter | Function |
| --- | --- |
| `Preset` | Preset selector. Pulse `Apply Preset` to apply. |
| `Apply Preset` | Applies the selected preset to all effect parameters and resets `RGB Adjust` to neutral. |
| `Amount` | Overall effect strength and blend influence. |
| `Speed` | Animation speed for jitter, noise, blocks, and scanlines. |
| `RGB Split` | Global horizontal separation between red, green, and blue samples. |
| `Scanlines` | Strength of animated horizontal scanline modulation. |
| `Jitter` | Density and distance of horizontal line displacement. |
| `Blocks` | Amount of block/tile displacement. |
| `Noise` | Strength of fine analog grain and cloudy noise. |
| `Posterize` | Reduces the number of color levels. |
| `Brightness` | Adds or subtracts overall brightness. |
| `Contrast` | Expands or compresses contrast around mid-gray. |
| `Tint` (x3) | Three separate channel multipliers — red, green, and blue — used for color grading. Cyberpunk's `(0.10, 0.78, 1.00)` produces the signature cyan look. |
| `Debug Mode` | Switches the output to intermediate shader data (`final` / `uv` / `noise` / `mask`). Return to `final` for normal output. |
| `Author` | Read-only author credit. |

Most controls use a 0-1 UI range, but they are not hard-clamped. Bundled presets may use `Speed` and `Contrast` above 1 or a small negative `Brightness` value.

## RGB Adjust controls

Per-channel geometric offsets applied on top of the global `RGB Split`. Use them to build asymmetric, hand-tuned chromatic aberration — for example shifting only the red channel horizontally while scaling it up.

| Parameter | Function |
| --- | --- |
| `Red Horizontal` / `Red Vertical` | Offsets the red channel horizontally / vertically (in UV units, e.g. `0.04` = 4% of the image). |
| `Red Scale` | Zooms the red channel around the image center. Values above `1` magnify. |
| `Green Horizontal` / `Green Vertical` / `Green Scale` | Same for the green channel. |
| `Blue Horizontal` / `Blue Vertical` / `Blue Scale` | Same for the blue channel. |

Neutral state is all offsets at `0` and all scales at `1`. Presets always reset this page to neutral, so apply the preset first, then adjust channels manually.

## Debug views

`Debug Mode` exposes intermediate shader data:

- `final`: finished effect.
- `uv`: displaced sampling coordinates.
- `noise`: animated procedural noise.
- `mask`: combined line, block, and tile activation mask.

Return to `final` for normal output.

## Quick recipes

### Clean RGB offset

- Start with `RGB_Split`.
- Lower `Noise`, `Blocks`, and `Scanlines`.
- Adjust `RGB Split` and `Amount`.
- For asymmetric separation, keep `RGB Split` low and offset single channels on the `RGB Adjust` page instead.

### Analog monitor

- Start with `Analog_Noise`.
- Raise `Scanlines` and `Noise`.
- Keep `RGB Split` subtle.

### Hard digital breakup

- Start with `Tile_Jitter` or `Datamosh_Lite`.
- Raise `Blocks`, `Jitter`, and `Posterize`.
- Use `Speed` to control motion intensity.

## Troubleshooting

| Problem | Adjustment |
| --- | --- |
| Effect is too chaotic | Lower `Amount`, `Jitter`, and `Blocks`. |
| Colors separate too far | Lower `RGB Split`, and check the `RGB Adjust` page for leftover channel offsets. |
| A single color channel is misaligned or zoomed | Reset the `RGB Adjust` page: offsets to `0`, scales to `1`. |
| Image is too noisy | Lower `Noise` and `Scanlines`. |
| Motion is too fast | Lower `Speed`. |
| Image becomes too dark or bright | Reset `Brightness`, then adjust `Contrast`. |
| Output shows UV/noise/mask data | Set `Debug Mode` to `final`. |
| Manual RGB Adjust tweaks disappeared | Applying a preset resets that page by design — re-apply channel offsets after the preset. |

## Technical notes

- One TOP input and one TOP output.
- GLSL TOP processing with input-driven resolution.
- Procedural hash/noise functions; no external texture dependency.
- Presets are stored as an internal CSV table and applied through the `Apply Preset` pulse.
- Built-in debug views (`final` / `uv` / `noise` / `mask`).

## Inspiration and implementation

The effect categories were inspired by [XanderXu/RealityGlitchArt](https://github.com/XanderXu/RealityGlitchArt), a Shader Graph example project for Reality Composer Pro / visionOS. This component is an independent TouchDesigner GLSL TOP rewrite for 2D image processing; it does not require RealityKit, Reality Composer Pro, or Apple platforms.
