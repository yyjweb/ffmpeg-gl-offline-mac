# Build from Source

This document explains how to compile FFmpeg with gl-transitions support from source on macOS.

## Prerequisites

### System Requirements

- macOS 12.0 (Monterey) or later
- Apple Silicon Mac (M1/M2/M3/M4/M5)
- Xcode Command Line Tools

### Install Command Line Tools

```bash
xcode-select --install
```

## Quick Build

```bash
# Clone the build repository
git clone https://github.com/YOUR_USERNAME/ffmpeg-gl-build.git
cd ffmpeg-gl-build

# Run the automated build script
./build-static.sh
```

The build process takes approximately 15-30 minutes depending on your Mac.

## Manual Build Steps

If you want to understand or customize the build process:

### 1. Download FFmpeg Source

```bash
wget https://ffmpeg.org/releases/ffmpeg-8.1.tar.xz
tar xf ffmpeg-8.1.tar.xz
cd ffmpeg-8.1
```

### 2. Download gl-transitions

```bash
git clone https://github.com/gl-transitions/gl-transitions.git
```

### 3. Create the Filter

Create `libavfilter/vf_gltransition.c` with the OpenGL transition filter implementation.

### 4. Modify Build Files

Add the filter to:
- `libavfilter/allfilters.c`
- `libavfilter/Makefile`

### 5. Configure and Build

```bash
./configure \
  --enable-static \
  --disable-shared \
  --enable-gpl \
  --enable-libx264 \
  --disable-videotoolbox \
  --disable-securetransport \
  --prefix=/path/to/install

make -j$(sysctl -n hw.ncpu)
make install
```

## Build Configuration

Our build uses these key options:

| Option | Purpose |
|--------|---------|
| `--enable-static` | Create static binary |
| `--disable-shared` | No dynamic libraries |
| `--enable-gpl` | Enable GPL-licensed code |
| `--enable-libx264` | Enable H.264 encoder |
| `--disable-videotoolbox` | Avoid VideoToolbox framework |
| `--disable-securetransport` | Avoid SecureTransport framework |

## Verify the Build

```bash
# Check binary
file bin/ffmpeg
# Should show: Mach-O 64-bit executable arm64

# Check dependencies
otool -L bin/ffmpeg
# Should only show system frameworks:
#   /System/Library/Frameworks/...

# Check gltransition filter
./bin/ffmpeg -filters | grep gltransition
# Should show: T. gltransition VV->V OpenGL blend transitions
```

## Customization

### Add More Codecs

```bash
./configure \
  --enable-static \
  --enable-gpl \
  --enable-libx264 \
  --enable-libx265 \
  --enable-libvpx \
  ...
```

### Optimize for Size

```bash
./configure \
  --enable-static \
  --disable-debug \
  --disable-doc \
  --disable-htmlpages \
  --disable-manpages \
  --disable-podpages \
  --disable-txtpages \
  ...
```

### Enable Hardware Acceleration

```bash
./configure \
  --enable-static \
  --enable-videotoolbox \
  --enable-audiotoolbox \
  ...
```

Note: This will add dependencies on macOS frameworks.

## Troubleshooting

### Build Fails with OpenGL Errors

Make sure you're using the correct OpenGL headers:

```bash
# Check OpenGL framework
ls /System/Library/Frameworks/OpenGL.framework/
```

### Missing gl-transitions Shaders

Copy the shaders to your output directory:

```bash
cp -r gl-transitions/transitions/*.glsl /path/to/output/transitions/
```

### Binary Has External Dependencies

Check what's linked:

```bash
otool -L bin/ffmpeg | grep -v "System/Library"
```

Remove any Homebrew or custom library dependencies by rebuilding with `--disable-shared` and ensuring no pkg-config paths point to external libraries.

## Build Script Reference

The complete build script handles:

1. Downloading FFmpeg source
2. Downloading gl-transitions
3. Patching FFmpeg with the gltransition filter
4. Configuring for static linking
5. Compiling with all CPU cores
6. Installing to output directory
7. Verifying the build
8. Creating the offline package

See `build-static.sh` in the build repository for the full implementation.

## Resources

- [FFmpeg Compilation Guide](https://trac.ffmpeg.org/wiki/CompilationGuide)
- [FFmpeg macOS Compilation](https://trac.ffmpeg.org/wiki/CompilationGuide/macOS)
- [gl-transitions Documentation](https://gl-transitions.com/)
- [OpenGL on macOS](https://developer.apple.com/documentation/opengl)

---

Need help? [Open an issue](https://github.com/YOUR_USERNAME/ffmpeg-gl-transitions-macos/issues)
