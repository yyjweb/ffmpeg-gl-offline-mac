# FFmpeg GL Transitions for macOS

<div align="center">

![FFmpeg](https://img.shields.io/badge/FFmpeg-8.1-green?logo=ffmpeg)
![Platform](https://img.shields.io/badge/Platform-Apple%20Silicon-blue?logo=apple)
![License](https://img.shields.io/badge/License-GPL%20v2%2B-orange)
![Transitions](https://img.shields.io/badge/Transitions-109-purple)

**🇺🇸 English** | [🇨🇳 中文](README_CN.md)

**A statically-linked FFmpeg 8.1 binary with 109 GPU-accelerated GLSL video transitions for Apple Silicon (M1/M2/M3/M4/M5)**

[Features](#-features) • [Quick Start](#-quick-start) • [Transitions](#-transitions) • [Usage](#-usage) • [Build](#-build-from-source)

</div>

---

## ✨ Features

- ✅ **Zero Dependencies** - Fully static linking, no external libraries needed
- ✅ **Apple Silicon Native** - Optimized for M1/M2/M3/M4/M5 (ARM64)
- ✅ **109 Transitions** - Complete gl-transitions library included
- ✅ **Offline Ready** - Download and run, no network required
- ✅ **GPU Accelerated** - Uses macOS Core OpenGL for hardware rendering
- ✅ **FFmpeg 8.1** - Latest FFmpeg version with all codecs

## 🚀 Quick Start

### Download

```bash
# Clone the repository
git clone https://github.com/yyjweb/ffmpeg-gl-offline.git
cd ffmpeg-gl-offline

# Verify the binary
./bin/ffmpeg -version
```

### Basic Usage

```bash
# Simple fade transition (default)
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2" \
  -c:v libx264 output.mp4

# Use a specific transition effect
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 output.mp4
```

### List All Transitions

```bash
ls -1 transitions/*.glsl | sed 's/.*\///' | sed 's/\.glsl//'
```

## 🎬 Transitions

| Category | Effects |
|----------|---------|
| **Geometric** | Cube, Fold, Flip, Rotate, Spiral, Doorway, Windowblinds |
| **Dissolve** | Fade, CrossWarp, Mosaic, Pixelize, CircleCrop |
| **Wipe** | WipeLeft, WipeRight, WipeUp, WipeDown, LinearBlur |
| **Zoom** | ZoomIn, ZoomOut, ZoomInCircles, SimpleZoom |
| **Special** | Burn, Glitch, Heart, Jelly, Ripple, WaterDrop |

**Total: 109 unique transitions**

Preview all transitions at [gl-transitions.com](https://gl-transitions.com/)

## 📖 Usage

### Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `duration` | Transition duration in seconds | 1.0 |
| `source` | Path to GLSL shader file | built-in fade |
| `offset` | Start offset for transition | 0.0 |

### Examples

#### Multiple Video Concatenation

```bash
./bin/ffmpeg -i intro.mp4 -i main.mp4 -i outro.mp4 \
  -filter_complex \
    "[0:v][1:v]gltransition=duration=2[v1]; \
     [v1][2:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 -preset fast -crf 23 final.mp4
```

#### High Quality Output

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=3:source=$(pwd)/transitions/crosswarp.glsl" \
  -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k output.mp4
```

#### Fast Encoding

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=1" \
  -c:v libx264 -preset ultrafast -crf 28 output.mp4
```

## 💻 System Requirements

- macOS 12.0 (Monterey) or later
- Apple Silicon Mac (M1/M2/M3/M4/M5)

**Note:** Intel Macs are not supported. For Intel Macs, you need to compile from source.

## 📦 Project Structure

```
ffmpeg-gl-offline/
├── bin/
│   └── ffmpeg              # Static binary (~22MB)
├── transitions/            # 109 GLSL shaders (~440KB)
│   ├── cube.glsl
│   ├── fade.glsl
│   ├── crosswarp.glsl
│   └── ...
├── LICENSE
├── README.md
└── README_CN.md
```

## 🔧 Build from Source

If you want to compile your own binary or customize the build:

### Prerequisites

```bash
# Install Xcode Command Line Tools
xcode-select --install

# The build uses system frameworks only
# No Homebrew dependencies required
```

### Build Steps

```bash
# Clone the build repository
git clone https://github.com/yyjweb/ffmpeg-gl-build.git
cd ffmpeg-gl-build

# Run the build script
./build-static.sh

# The compiled binary will be in:
# ffmpeg-gl-offline/bin/ffmpeg
```

See [BUILD.md](BUILD.md) for detailed compilation instructions.

## 🐛 Troubleshooting

### "shader file not found"

Use absolute paths or paths relative to your current directory:

```bash
# ❌ Wrong
gltransition=source=cube.glsl

# ✅ Correct
gltransition=source=$(pwd)/transitions/cube.glsl
gltransition=source=/full/path/to/transitions/cube.glsl
```

### "No transition effect in output"

Ensure both input videos have valid video streams:

```bash
./bin/ffmpeg -i video1.mp4  # Check for "Stream #0:0: Video"
```

### Slow Encoding

Use faster presets:

```bash
-preset fast      # Good balance
-preset ultrafast # Fastest encoding
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the **GNU General Public License v2.0 or later** due to the inclusion of:

- FFmpeg (LGPL v2.1+, with GPL v2+ components)
- libx264 (GPL v2+)

The GLSL transition shaders in `transitions/` are from [gl-transitions](https://github.com/gl-transitions/gl-transitions) and are licensed under the MIT License.

See [LICENSE](LICENSE) for full details.

## 🙏 Acknowledgments

- [FFmpeg](https://ffmpeg.org/) - The ultimate multimedia framework
- [gl-transitions](https://gl-transitions.com/) - Beautiful GLSL transition library
- [gl-transition example filter](https://github.com/gl-transitions/gl-transition) - Reference implementation

## 📊 Project Stats

![GitHub stars](https://img.shields.io/github/stars/yyjweb/ffmpeg-gl-offline?style=social)
![GitHub forks](https://img.shields.io/github/forks/yyjweb/ffmpeg-gl-offline?style=social)
![GitHub issues](https://img.shields.io/github/issues/yyjweb/ffmpeg-gl-offline)

---

<div align="center">

**Made with ❤️ for the macOS video editing community**

[⬆ Back to Top](#ffmpeg-gl-transitions-for-macos)

</div>
