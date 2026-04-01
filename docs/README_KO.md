# FFmpeg GL Transitions - macOS

<div align="center">

![FFmpeg](https://img.shields.io/badge/FFmpeg-8.1-green?logo=ffmpeg)
![플랫폼](https://img.shields.io/badge/플랫폼-Apple%20Silicon-blue?logo=apple)
![라이선스](https://img.shields.io/badge/라이선스-GPL%20v2%2B-orange)
![트랜지션](https://img.shields.io/badge/트랜지션-109-purple)

[🇺🇸 English](../README.md) | [🇨🇳 中文](README_CN.md) | [🇯🇵 日本語](README_JA.md) | **🇰🇷 한국어** | [🇪🇸 Español](README_ES.md) | [🇫🇷 Français](README_FR.md) | [🇷🇺 Русский](README_RU.md) | [🇧🇷 Português](README_PT.md)

**Apple Silicon (M1/M2/M3/M4/M5)용으로 컴파일된 FFmpeg 8.1 정적 바이너리, 109개의 GPU 가속 GLSL 비디오 트랜지션 내장**

[기능](#-기능) • [빠른 시작](#-빠른-시작) • [트랜지션](#-트랜지션) • [사용법](#-사용법)

</div>

---

## ✨ 기능

- ✅ **종속성 없음** - 완전 정적 링크, 외부 라이브러리 불필요
- ✅ **Apple Silicon 네이티브** - M1/M2/M3/M4/M5 (ARM64) 최적화
- ✅ **109개 트랜지션** - gl-transitions 라이브러리 전체 포함
- ✅ **오프라인 지원** - 다운로드 후 바로 사용, 네트워크 불필요
- ✅ **GPU 가속** - macOS Core OpenGL 하드웨어 렌더링 사용
- ✅ **FFmpeg 8.1** - 최신 FFmpeg 버전, 모든 코덱 지원

## 🚀 빠른 시작

### 다운로드

```bash
# 저장소 클론
git clone https://github.com/yyjweb/ffmpeg-gl-offline-mac.git
cd ffmpeg-gl-offline-mac

# 바이너리 확인
./bin/ffmpeg -version
```

### 기본 사용법

```bash
# 간단한 페이드 트랜지션 (기본값)
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2" \
  -c:v libx264 output.mp4

# 특정 트랜지션 효과 사용
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 output.mp4
```

### 모든 트랜지션 나열

```bash
ls -1 transitions/*.glsl | sed 's/.*\///' | sed 's/\.glsl//'
```

## 🎬 트랜지션

| 카테고리 | 효과 |
|----------|------|
| **지오메트릭** | Cube, Fold, Flip, Rotate, Spiral, Doorway, Windowblinds |
| **디졸브** | Fade, CrossWarp, Mosaic, Pixelize, CircleCrop |
| **와이프** | WipeLeft, WipeRight, WipeUp, WipeDown, LinearBlur |
| **줌** | ZoomIn, ZoomOut, ZoomInCircles, SimpleZoom |
| **스페셜** | Burn, Glitch, Heart, Jelly, Ripple, WaterDrop |

**총 109개의 독특한 트랜지션**

모든 트랜지션 미리보기: [gl-transitions.com](https://gl-transitions.com/)

## 📖 사용법

### 매개변수

| 매개변수 | 설명 | 기본값 |
|---------|------|--------|
| `duration` | 트랜지션 지속 시간 (초) | 1.0 |
| `source` | GLSL 셰이더 파일 경로 | 내장 페이드 |
| `offset` | 트랜지션 시작 오프셋 | 0.0 |

### 예제

#### 여러 동영상 연결

```bash
./bin/ffmpeg -i intro.mp4 -i main.mp4 -i outro.mp4 \
  -filter_complex \
    "[0:v][1:v]gltransition=duration=2[v1]; \
     [v1][2:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 -preset fast -crf 23 final.mp4
```

#### 고품질 출력

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=3:source=$(pwd)/transitions/crosswarp.glsl" \
  -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k output.mp4
```

#### 빠른 인코딩

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=1" \
  -c:v libx264 -preset ultrafast -crf 28 output.mp4
```

## 💻 시스템 요구사항

- macOS 12.0 (Monterey) 이상
- Apple Silicon Mac (M1/M2/M3/M4/M5)

**참고:** Intel Mac은 지원되지 않습니다.

## 📦 프로젝트 구조

```
ffmpeg-gl-offline-mac/
├── bin/
│   └── ffmpeg              # 정적 바이너리 (~22MB)
├── transitions/            # 109개의 GLSL 셰이더 (~440KB)
│   ├── cube.glsl
│   ├── fade.glsl
│   ├── crosswarp.glsl
│   └── ...
├── LICENSE
└── README.md
```

## 🐛 문제 해결

### "shader file not found"

절대 경로 또는 현재 디렉토리에서의 상대 경로를 사용하세요:

```bash
# ❌ 잘못됨
gltransition=source=cube.glsl

# ✅ 올바름
gltransition=source=$(pwd)/transitions/cube.glsl
gltransition=source=/full/path/to/transitions/cube.glsl
```

### "출력에 트랜지션 효과가 없음"

두 입력 동영상 모두 유효한 비디오 스트림이 있는지 확인:

```bash
./bin/ffmpeg -i video1.mp4  # "Stream #0:0: Video" 확인
```

### 인코딩이 느림

더 빠른 프리셋 사용:

```bash
-preset fast      # 균형 잡힌 속도와 품질
-preset ultrafast # 가장 빠른 인코딩
```

## 🤝 기여

기여를 환영합니다! 자유롭게 Pull Request를 제출해 주세요.

1. 저장소 포크
2. 기능 브랜치 생성 (`git checkout -b feature/AmazingFeature`)
3. 변경 사항 커밋 (`git commit -m 'Add some AmazingFeature'`)
4. 브랜치에 푸시 (`git push origin feature/AmazingFeature`)
5. Pull Request 생성

## 📄 라이선스

이 프로젝트는 다음 구성 요소를 포함하므로 **GNU General Public License v2.0 이상**으로 라이선스됩니다:

- FFmpeg (LGPL v2.1+, GPL v2+ 구성 요소 포함)
- libx264 (GPL v2+)

`transitions/`의 GLSL 트랜지션 셰이더는 [gl-transitions](https://github.com/gl-transitions/gl-transitions)에서 가져왔으며 MIT 라이선스입니다.

자세한 내용은 [LICENSE](../LICENSE)를 참조하세요.

## 🙏 감사의 말

- [FFmpeg](https://ffmpeg.org/) - 궁극의 멀티미디어 프레임워크
- [gl-transitions](https://gl-transitions.com/) - 아름다운 GLSL 트랜지션 라이브러리
- [gl-transition example filter](https://github.com/gl-transitions/gl-transition) - 참조 구현

## 📊 프로젝트 통계

![GitHub stars](https://img.shields.io/github/stars/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub forks](https://img.shields.io/github/forks/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub issues](https://img.shields.io/github/issues/yyjweb/ffmpeg-gl-offline-mac)

---

<div align="center">

**macOS 비디오 편집 커뮤니티를 위해 ❤️로 제작**

[⬆ 맨 위로](#ffmpeg-gl-transitions---macos)

</div>
