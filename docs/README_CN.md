# FFmpeg GL Transitions - macOS 版

<div align="center">

![FFmpeg](https://img.shields.io/badge/FFmpeg-8.1-green?logo=ffmpeg)
![平台](https://img.shields.io/badge/平台-Apple%20Silicon-blue?logo=apple)
![许可证](https://img.shields.io/badge/许可证-GPL%20v2%2B-orange)
![转场特效](https://img.shields.io/badge/转场特效-109-purple)

[🇺🇸 English](../README.md) | **🇨🇳 中文** | [🇯🇵 日本語](README_JA.md) | [🇰🇷 한국어](README_KO.md) | [🇪🇸 Español](README_ES.md) | [🇫🇷 Français](README_FR.md) | [🇷🇺 Русский](README_RU.md) | [🇧🇷 Português](README_PT.md)

**为 Apple Silicon (M1/M2/M3/M4/M5) 编译的 FFmpeg 8.1 静态版本，内置 109 种 GPU 加速的 GLSL 视频转场特效**

[功能特性](#-功能特性) • [快速开始](#-快速开始) • [转场特效](#-转场特效) • [使用方法](#-使用方法)

</div>

---

## ✨ 功能特性

- ✅ **零依赖** - 完全静态链接，无需安装任何外部库
- ✅ **Apple Silicon 原生** - 针对 M1/M2/M3/M4/M5 (ARM64) 优化
- ✅ **109 种转场** - 包含完整的 gl-transitions 特效库
- ✅ **离线可用** - 下载即用，无需网络
- ✅ **GPU 加速** - 使用 macOS Core OpenGL 硬件渲染
- ✅ **FFmpeg 8.1** - 最新版本的 FFmpeg，支持所有主流编解码器

## 🚀 快速开始

### 下载

```bash
# 克隆仓库
git clone https://github.com/yyjweb/ffmpeg-gl-offline-mac.git
cd ffmpeg-gl-offline-mac

# 验证二进制文件
./bin/ffmpeg -version
```

### 基础用法

```bash
# 简单淡入淡出转场（默认）
./bin/ffmpeg -i 视频1.mp4 -i 视频2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2" \
  -c:v libx264 输出.mp4

# 使用特定的转场效果
./bin/ffmpeg -i 视频1.mp4 -i 视频2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 输出.mp4
```

### 列出所有转场

```bash
ls -1 transitions/*.glsl | sed 's/.*\///' | sed 's/\.glsl//'
```

## 🎬 转场特效

| 类别 | 效果示例 |
|------|----------|
| **几何变换** | Cube (立方体), Fold (折叠), Flip (翻转), Rotate (旋转), Spiral (螺旋) |
| **溶解** | Fade (淡入淡出), CrossWarp (交叉扭曲), Mosaic (马赛克), Pixelize (像素化) |
| **擦除** | WipeLeft, WipeRight, WipeUp, WipeDown, LinearBlur |
| **缩放** | ZoomIn, ZoomOut, ZoomInCircles, SimpleZoom |
| **特效** | Burn (燃烧), Glitch (故障), Heart (心形), Jelly (果冻), Ripple (涟漪) |

**总计：109 种独特转场**

预览所有转场效果：[gl-transitions.com](https://gl-transitions.com/)

## 📖 使用方法

### 参数说明

| 参数 | 描述 | 默认值 |
|------|------|--------|
| `duration` | 转场持续时间（秒） | 1.0 |
| `source` | GLSL 着色器文件路径 | 内置淡入淡出 |
| `offset` | 转场开始偏移量 | 0.0 |

### 示例

#### 多视频拼接

```bash
./bin/ffmpeg -i 片头.mp4 -i 主体.mp4 -i 片尾.mp4 \
  -filter_complex \
    "[0:v][1:v]gltransition=duration=2[v1]; \
     [v1][2:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 -preset fast -crf 23 最终输出.mp4
```

#### 高质量输出

```bash
./bin/ffmpeg -i 视频1.mp4 -i 视频2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=3:source=$(pwd)/transitions/crosswarp.glsl" \
  -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k 输出.mp4
```

#### 快速编码

```bash
./bin/ffmpeg -i 视频1.mp4 -i 视频2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=1" \
  -c:v libx264 -preset ultrafast -crf 28 输出.mp4
```

## 💻 系统要求

- macOS 12.0 (Monterey) 或更高版本
- Apple Silicon Mac (M1/M2/M3/M4/M5)

**注意：** 不支持 Intel Mac。

## 📦 项目结构

```
ffmpeg-gl-offline-mac/
├── bin/
│   └── ffmpeg              # 静态二进制文件 (~22MB)
├── transitions/            # 109 个 GLSL 着色器 (~440KB)
│   ├── cube.glsl
│   ├── fade.glsl
│   ├── crosswarp.glsl
│   └── ...
├── LICENSE
└── README.md
```

## 🐛 故障排除

### "shader file not found"（着色器文件未找到）

使用绝对路径或相对于当前目录的路径：

```bash
# ❌ 错误
gltransition=source=cube.glsl

# ✅ 正确
gltransition=source=$(pwd)/transitions/cube.glsl
gltransition=source=/完整路径/transitions/cube.glsl
```

### "输出视频没有转场效果"

确保两个输入视频都有有效的视频流：

```bash
./bin/ffmpeg -i 视频1.mp4  # 检查是否有 "Stream #0:0: Video"
```

### 编码速度慢

使用更快的预设：

```bash
-preset fast      # 平衡速度和质量
-preset ultrafast # 最快编码速度
```

## 🤝 贡献

欢迎贡献代码！请随时提交 Pull Request。

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m '添加某个特性'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 Pull Request

## 📄 许可证

本项目基于 **GNU General Public License v2.0 或更高版本** 许可，因为包含以下组件：

- FFmpeg (LGPL v2.1+, 包含 GPL v2+ 组件)
- libx264 (GPL v2+)

`transitions/` 目录中的 GLSL 转场着色器来自 [gl-transitions](https://github.com/gl-transitions/gl-transitions)，采用 MIT 许可证。

详见 [LICENSE](../LICENSE)。

## 🙏 致谢

- [FFmpeg](https://ffmpeg.org/) - 终极多媒体框架
- [gl-transitions](https://gl-transitions.com/) - 精美的 GLSL 转场库
- [gl-transition example filter](https://github.com/gl-transitions/gl-transition) - 参考实现

## 📊 项目统计

![GitHub stars](https://img.shields.io/github/stars/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub forks](https://img.shields.io/github/forks/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub issues](https://img.shields.io/github/issues/yyjweb/ffmpeg-gl-offline-mac)

---

<div align="center">

**为 macOS 视频编辑社区用 ❤️ 制作**

[⬆ 返回顶部](#ffmpeg-gl-transitions---macos-版)

</div>
