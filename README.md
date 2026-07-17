# Uinipan TouchDesigner Plugins

English | [中文](./README.zh-CN.md)

A small collection of reusable TouchDesigner `.tox` components for audio-reactive control, GLSL glitch processing, interactive masking, and particle rendering.

Author: `uinipan`

## Plugins

| Plugin | Type | Description |
| --- | --- | --- |
| [Audio Reactive Tools](./plugin_audio_reactive_tools/) | CHOP | Converts one audio input into envelope, accumulated speed, kick/snare triggers, and counts. |
| [Glitch Line Filter](./plugin_glitch_line_filter/) | TOP / GLSL | Converts images into broken horizontal monochrome sketch lines. |
| [Image Particles](./plugin_image_particles/) | TOP / SOP / Render | Converts images or video into animated colored particles. |
| [Mouse Reveal Feedback](./plugin_mouse_reveal_feedback/) | TOP / Feedback | Paints a persistent or fading reveal/erase mask with the mouse. |
| [Reality Glitch Filter](./plugin_reality_glitch_filter/) | TOP / GLSL | Combines RGB split, scanlines, jitter, block displacement, analog noise, and posterization. |

Each plugin folder contains:

- the packaged `.tox` component;
- an English `README.md`;
- a Chinese `README.zh-CN.md`;
- preview images when relevant.

## Requirements

- TouchDesigner 2023 or newer.
- The packaged components were documented and tested with TouchDesigner 2023.11280 on Windows.
- A compatible GPU is recommended for GLSL and particle components.

## Installation

1. Download or clone this repository.
2. Open the folder for the plugin you want.
3. Drag its `.tox` file into a TouchDesigner project.
4. Follow the plugin-specific README for inputs, outputs, controls, and troubleshooting.

## Repository structure

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

## Notes

- The components are designed to be used as packaged tools. Internal networks can be inspected for learning and customization.
- Output resolution behavior is documented in each plugin folder.
- The `Reality Glitch Filter` README includes attribution for its visual concept inspiration.

## License

Released under the [MIT License](./LICENSE). Commercial use is permitted under the license terms.
