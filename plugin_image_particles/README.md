# Image Particles

English | [中文](./README.zh-CN.md)

`image_particles.tox` converts an image, video, or other TOP into an animated colored particle rendering while preserving the source image's aspect ratio and color information.

Author: `uinipan`

## Compatibility

Tested with TouchDesigner 2023.11280 on Windows.

## Installation

1. Drag `image_particles.tox` into a TouchDesigner project.
2. Connect an image, Movie File In TOP, or another TOP to its input.
3. Use the component output as the particle TOP.
4. Adjust the `Image Particles` parameter page.

```text
Image or video TOP -> image_particles -> particle TOP output
```

The output has a transparent background for downstream compositing.

## Quick start

1. Use `Brightness Threshold` to choose which source regions generate particles.
2. Adjust particle thickness with `Point Size`.
3. Add depth with `Image Depth`.
4. Add motion with `Noise Amount` and `Noise Speed`.
5. Fit the subject with `Image Scale`.
6. Choose a final pixel size with `Max Render Resolution`.

## Controls

| Parameter | Range | Function |
| --- | ---: | --- |
| `Brightness Threshold` | 0-1 | Filters source pixels by brightness. Higher values remove more dark areas. |
| `Point Size` | 0.005-0.2 | Controls the size of each rendered particle. |
| `Image Depth` | 0-5 | Adds front-to-back depth. Set to 0 for a flatter result. |
| `Noise Amount` | 0-2 | Controls particle displacement strength. |
| `Noise Speed` | -3-3 | Controls noise animation speed; negative values reverse direction. |
| `Image Scale` | 0.25-4 | Controls how large the particle image appears in the frame. |
| `Max Render Resolution` | 256-4096 | Sets the longest output edge in pixels. |
| `Actual Render Resolution` | Read-only | Displays the current calculated width and height. |

## Aspect ratio and resolution

`Max Render Resolution` controls the longest edge instead of forcing a fixed width and height:

- Landscape input: width uses the selected maximum; height is calculated from the input ratio.
- Portrait input: height uses the selected maximum; width is calculated from the input ratio.
- Square input: width and height are equal.

For a `2080 × 3120` portrait input:

- Maximum 2048 produces approximately `1365 × 2048`.
- Maximum 4096 produces approximately `2731 × 4096`.

Use `Image Scale` for composition and `Max Render Resolution` for pixel clarity/performance. Resolution should not be used to resize the subject within the frame.

## Performance

- Use 1280 or 1920 for normal preview work.
- Raise the maximum to 2048 or 4096 only for high-resolution output.
- Lower `Max Render Resolution` first when playback becomes slow.
- Increasing render resolution improves output pixels but does not increase particle sampling density.

## Troubleshooting

| Problem | Adjustment |
| --- | --- |
| Too few particles or missing subject | Lower `Brightness Threshold`. |
| Particles merge into a solid shape | Lower `Point Size`. |
| Result looks flat | Raise `Image Depth`. |
| Motion is too chaotic | Lower `Noise Amount` or `Noise Speed`. |
| Subject is too large or small | Adjust `Image Scale`. |
| Output looks soft | Raise `Max Render Resolution`. |
| High-resolution playback is slow | Lower `Max Render Resolution`. |

## Known note

The internal `image_points` Script SOP may show a yellow cook dependency loop warning. The packaged version was tested with correct visual output despite this warning.

