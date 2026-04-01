# FFmpeg GL Transitions para macOS

<div align="center">

![FFmpeg](https://img.shields.io/badge/FFmpeg-8.1-green?logo=ffmpeg)
![Plataforma](https://img.shields.io/badge/Plataforma-Apple%20Silicon-blue?logo=apple)
![Licença](https://img.shields.io/badge/Licença-GPL%20v2%2B-orange)
![Transições](https://img.shields.io/badge/Transições-109-purple)

[🇺🇸 English](../README.md) | [🇨🇳 中文](README_CN.md) | [🇯🇵 日本語](README_JA.md) | [🇰🇷 한국어](README_KO.md) | [🇪🇸 Español](README_ES.md) | [🇫🇷 Français](README_FR.md) | [🇷🇺 Русский](README_RU.md) | **🇧🇷 Português**

**Binário estático do FFmpeg 8.1 com 109 transições de vídeo GLSL aceleradas por GPU para Apple Silicon (M1/M2/M3/M4/M5)**

[Recursos](#-recursos) • [Início Rápido](#-início-rápido) • [Transições](#-transições) • [Uso](#-uso)

</div>

---

## ✨ Recursos

- ✅ **Zero Dependências** - Linkagem estática completa, sem bibliotecas externas necessárias
- ✅ **Nativo Apple Silicon** - Otimizado para M1/M2/M3/M4/M5 (ARM64)
- ✅ **109 Transições** - Biblioteca completa gl-transitions incluída
- ✅ **Pronto Offline** - Baixe e execute, sem rede necessária
- ✅ **Aceleração GPU** - Usa macOS Core OpenGL para renderização por hardware
- ✅ **FFmpeg 8.1** - Última versão do FFmpeg com todos os codecs

## 🚀 Início Rápido

### Baixar

```bash
# Clonar o repositório
git clone https://github.com/yyjweb/ffmpeg-gl-offline-mac.git
cd ffmpeg-gl-offline-mac

# Verificar o binário
./bin/ffmpeg -version
```

### Uso Básico

```bash
# Transição de fade simples (padrão)
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2" \
  -c:v libx264 output.mp4

# Usar um efeito de transição específico
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 output.mp4
```

### Listar Todas as Transições

```bash
ls -1 transitions/*.glsl | sed 's/.*\///' | sed 's/\.glsl//'
```

## 🎬 Transições

| Categoria | Efeitos |
|-----------|---------|
| **Geométricas** | Cube, Fold, Flip, Rotate, Spiral, Doorway, Windowblinds |
| **Dissolver** | Fade, CrossWarp, Mosaic, Pixelize, CircleCrop |
| **Varredura** | WipeLeft, WipeRight, WipeUp, WipeDown, LinearBlur |
| **Zoom** | ZoomIn, ZoomOut, ZoomInCircles, SimpleZoom |
| **Especiais** | Burn, Glitch, Heart, Jelly, Ripple, WaterDrop |

**Total: 109 transições únicas**

Visualize todas as transições em [gl-transitions.com](https://gl-transitions.com/)

## 📖 Uso

### Parâmetros

| Parâmetro | Descrição | Padrão |
|-----------|-----------|--------|
| `duration` | Duração da transição em segundos | 1.0 |
| `source` | Caminho para o arquivo shader GLSL | fade embutido |
| `offset` | Deslocamento inicial da transição | 0.0 |

### Exemplos

#### Concatenação de Múltiplos Vídeos

```bash
./bin/ffmpeg -i intro.mp4 -i main.mp4 -i outro.mp4 \
  -filter_complex \
    "[0:v][1:v]gltransition=duration=2[v1]; \
     [v1][2:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 -preset fast -crf 23 final.mp4
```

#### Saída de Alta Qualidade

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=3:source=$(pwd)/transitions/crosswarp.glsl" \
  -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k output.mp4
```

#### Codificação Rápida

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=1" \
  -c:v libx264 -preset ultrafast -crf 28 output.mp4
```

## 💻 Requisitos do Sistema

- macOS 12.0 (Monterey) ou posterior
- Mac com Apple Silicon (M1/M2/M3/M4/M5)

**Nota:** Macs Intel não são suportados.

## 📦 Estrutura do Projeto

```
ffmpeg-gl-offline-mac/
├── bin/
│   └── ffmpeg              # Binário estático (~22MB)
├── transitions/            # 109 shaders GLSL (~440KB)
│   ├── cube.glsl
│   ├── fade.glsl
│   ├── crosswarp.glsl
│   └── ...
├── LICENSE
└── README.md
```

## 🐛 Solução de Problemas

### "shader file not found"

Use caminhos absolutos ou relativos ao seu diretório atual:

```bash
# ❌ Incorreto
gltransition=source=cube.glsl

# ✅ Correto
gltransition=source=$(pwd)/transitions/cube.glsl
gltransition=source=/caminho/completo/para/transitions/cube.glsl
```

### "Sem efeito de transição na saída"

Certifique-se de que ambos os vídeos de entrada tenham streams de vídeo válidos:

```bash
./bin/ffmpeg -i video1.mp4  # Verificar "Stream #0:0: Video"
```

### Codificação Lenta

Use predefinições mais rápidas:

```bash
-preset fast      # Bom equilíbrio
-preset ultrafast # Codificação mais rápida
```

## 🤝 Contribuindo

Contribuições são bem-vindas! Sinta-se à vontade para enviar um Pull Request.

1. Faça fork do repositório
2. Crie sua branch de funcionalidade (`git checkout -b feature/AmazingFeature`)
3. Confirme suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Envie para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está licenciado sob a **GNU General Public License v2.0 ou posterior** devido à inclusão de:

- FFmpeg (LGPL v2.1+, com componentes GPL v2+)
- libx264 (GPL v2+)

Os shaders de transição GLSL em `transitions/` são de [gl-transitions](https://github.com/gl-transitions/gl-transitions) e estão licenciados sob a Licença MIT.

Consulte [LICENSE](../LICENSE) para mais detalhes.

## 🙏 Agradecimentos

- [FFmpeg](https://ffmpeg.org/) - A estrutura multimídia definitiva
- [gl-transitions](https://gl-transitions.com/) - Bela biblioteca de transições GLSL
- [gl-transition example filter](https://github.com/gl-transitions/gl-transition) - Implementação de referência

## 📊 Estatísticas do Projeto

![GitHub stars](https://img.shields.io/github/stars/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub forks](https://img.shields.io/github/forks/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub issues](https://img.shields.io/github/issues/yyjweb/ffmpeg-gl-offline-mac)

---

<div align="center">

**Feito com ❤️ para a comunidade de edição de vídeo macOS**

[⬆ Voltar ao Topo](#ffmpeg-gl-transitions-para-macos)

</div>
