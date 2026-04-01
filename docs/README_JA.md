# FFmpeg GL Transitions - macOS版

<div align="center">

![FFmpeg](https://img.shields.io/badge/FFmpeg-8.1-green?logo=ffmpeg)
![プラットフォーム](https://img.shields.io/badge/プラットフォーム-Apple%20Silicon-blue?logo=apple)
![ライセンス](https://img.shields.io/badge/ライセンス-GPL%20v2%2B-orange)
![トランジション](https://img.shields.io/badge/トランジション-109-purple)

[🇺🇸 English](../README.md) | [🇨🇳 中文](README_CN.md) | **🇯🇵 日本語** | [🇰🇷 한국어](README_KO.md) | [🇪🇸 Español](README_ES.md) | [🇫🇷 Français](README_FR.md) | [🇷🇺 Русский](README_RU.md) | [🇧🇷 Português](README_PT.md)

**Apple Silicon (M1/M2/M3/M4/M5)向けにコンパイルされたFFmpeg 8.1スタティックバイナリ。109種類のGPU高速化GLSLビデオトランジションを内蔵**

[機能](#-機能) • [クイックスタート](#-クイックスタート) • [トランジション](#-トランジション) • [使い方](#-使い方)

</div>

---

## ✨ 機能

- ✅ **依存関係ゼロ** - 完全スタティックリンク、外部ライブラリ不要
- ✅ **Apple Siliconネイティブ** - M1/M2/M3/M4/M5 (ARM64)に最適化
- ✅ **109種類のトランジション** - gl-transitionsライブラリ完全版を収録
- ✅ **オフライン対応** - ダウンロードしてすぐ使える、ネットワーク不要
- ✅ **GPU高速化** - macOS Core OpenGLによるハードウェアレンダリング
- ✅ **FFmpeg 8.1** - 最新版FFmpeg、全コーデック対応

## 🚀 クイックスタート

### ダウンロード

```bash
# リポジトリをクローン
git clone https://github.com/yyjweb/ffmpeg-gl-offline-mac.git
cd ffmpeg-gl-offline-mac

# バイナリを確認
./bin/ffmpeg -version
```

### 基本的な使い方

```bash
# シンプルなフェードトランジション（デフォルト）
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2" \
  -c:v libx264 output.mp4

# 特定のトランジション効果を使用
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 output.mp4
```

### 全トランジション一覧

```bash
ls -1 transitions/*.glsl | sed 's/.*\///' | sed 's/\.glsl//'
```

## 🎬 トランジション

| カテゴリ | 効果 |
|----------|------|
| **ジオメトリック** | Cube, Fold, Flip, Rotate, Spiral, Doorway, Windowblinds |
| **ディゾルブ** | Fade, CrossWarp, Mosaic, Pixelize, CircleCrop |
| **ワイプ** | WipeLeft, WipeRight, WipeUp, WipeDown, LinearBlur |
| **ズーム** | ZoomIn, ZoomOut, ZoomInCircles, SimpleZoom |
| **スペシャル** | Burn, Glitch, Heart, Jelly, Ripple, WaterDrop |

**合計：109種類のユニークなトランジション**

全トランジションのプレビュー：[gl-transitions.com](https://gl-transitions.com/)

## 📖 使い方

### パラメータ

| パラメータ | 説明 | デフォルト値 |
|-----------|------|-------------|
| `duration` | トランジションの長さ（秒） | 1.0 |
| `source` | GLSLシェーダーファイルへのパス | 内蔵フェード |
| `offset` | トランジション開始オフセット | 0.0 |

### 例

#### 複数動画の結合

```bash
./bin/ffmpeg -i intro.mp4 -i main.mp4 -i outro.mp4 \
  -filter_complex \
    "[0:v][1:v]gltransition=duration=2[v1]; \
     [v1][2:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 -preset fast -crf 23 final.mp4
```

#### 高品質出力

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=3:source=$(pwd)/transitions/crosswarp.glsl" \
  -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k output.mp4
```

#### 高速エンコード

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=1" \
  -c:v libx264 -preset ultrafast -crf 28 output.mp4
```

## 💻 システム要件

- macOS 12.0 (Monterey) 以降
- Apple Silicon Mac (M1/M2/M3/M4/M5)

**注意：** Intel Macには対応していません。

## 📦 プロジェクト構成

```
ffmpeg-gl-offline-mac/
├── bin/
│   └── ffmpeg              # スタティックバイナリ (~22MB)
├── transitions/            # 109個のGLSLシェーダー (~440KB)
│   ├── cube.glsl
│   ├── fade.glsl
│   ├── crosswarp.glsl
│   └── ...
├── LICENSE
└── README.md
```

## 🐛 トラブルシューティング

### "shader file not found"

絶対パスまたは現在のディレクトリからの相対パスを使用してください：

```bash
# ❌ 間違い
gltransition=source=cube.glsl

# ✅ 正しい
gltransition=source=$(pwd)/transitions/cube.glsl
gltransition=source=/full/path/to/transitions/cube.glsl
```

### "出力にトランジション効果がない"

両方の入力動画に有効なビデオストリームがあることを確認：

```bash
./bin/ffmpeg -i video1.mp4  # "Stream #0:0: Video"を確認
```

### エンコードが遅い

より高速なプリセットを使用：

```bash
-preset fast      # バランス重視
-preset ultrafast # 最速エンコード
```

## 🤝 コントリビュート

コントリビュートは大歓迎です！お気軽にPull Requestを提出してください。

1. リポジトリをフォーク
2. 機能ブランチを作成 (`git checkout -b feature/AmazingFeature`)
3. 変更をコミット (`git commit -m 'Add some AmazingFeature'`)
4. ブランチにプッシュ (`git push origin feature/AmazingFeature`)
5. Pull Requestを作成

## 📄 ライセンス

このプロジェクトは以下のコンポーネントを含むため、**GNU General Public License v2.0以降**でライセンスされています：

- FFmpeg (LGPL v2.1+, GPL v2+コンポーネントを含む)
- libx264 (GPL v2+)

`transitions/`のGLSLトランジションシェーダーは[gl-transitions](https://github.com/gl-transitions/gl-transitions)からのもので、MITライセンスです。

詳細は[LICENSE](../LICENSE)をご覧ください。

## 🙏 謝辞

- [FFmpeg](https://ffmpeg.org/) - 究極のマルチメディアフレームワーク
- [gl-transitions](https://gl-transitions.com/) - 美しいGLSLトランジションライブラリ
- [gl-transition example filter](https://github.com/gl-transitions/gl-transition) - リファレンス実装

## 📊 プロジェクト統計

![GitHub stars](https://img.shields.io/github/stars/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub forks](https://img.shields.io/github/forks/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub issues](https://img.shields.io/github/issues/yyjweb/ffmpeg-gl-offline-mac)

---

<div align="center">

**macOSビデオ編集コミュニティのために❤️を込めて作成**

[⬆ トップに戻る](#ffmpeg-gl-transitions---macos版)

</div>
