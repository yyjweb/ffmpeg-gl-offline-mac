# FFmpeg GL Transitions para macOS

<div align="center">

![FFmpeg](https://img.shields.io/badge/FFmpeg-8.1-green?logo=ffmpeg)
![Plataforma](https://img.shields.io/badge/Plataforma-Apple%20Silicon-blue?logo=apple)
![Licencia](https://img.shields.io/badge/Licencia-GPL%20v2%2B-orange)
![Transiciones](https://img.shields.io/badge/Transiciones-109-purple)

[🇺🇸 English](../README.md) | [🇨🇳 中文](README_CN.md) | [🇯🇵 日本語](README_JA.md) | [🇰🇷 한국어](README_KO.md) | **🇪🇸 Español** | [🇫🇷 Français](README_FR.md) | [🇷🇺 Русский](README_RU.md) | [🇧🇷 Português](README_PT.md)

**Binario estático de FFmpeg 8.1 con 109 transiciones de video GLSL aceleradas por GPU para Apple Silicon (M1/M2/M3/M4/M5)**

[Características](#-características) • [Inicio Rápido](#-inicio-rápido) • [Transiciones](#-transiciones) • [Uso](#-uso)

</div>

---

## ✨ Características

- ✅ **Sin Dependencias** - Enlace estático completo, sin bibliotecas externas necesarias
- ✅ **Nativo Apple Silicon** - Optimizado para M1/M2/M3/M4/M5 (ARM64)
- ✅ **109 Transiciones** - Biblioteca completa de gl-transitions incluida
- ✅ **Listo para Uso Offline** - Descarga y ejecuta, sin necesidad de red
- ✅ **Aceleración GPU** - Usa macOS Core OpenGL para renderizado por hardware
- ✅ **FFmpeg 8.1** - Última versión de FFmpeg con todos los códecs

## 🚀 Inicio Rápido

### Descargar

```bash
# Clonar el repositorio
git clone https://github.com/yyjweb/ffmpeg-gl-offline-mac.git
cd ffmpeg-gl-offline-mac

# Verificar el binario
./bin/ffmpeg -version
```

### Uso Básico

```bash
# Transición de fundido simple (predeterminada)
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2" \
  -c:v libx264 output.mp4

# Usar un efecto de transición específico
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 output.mp4
```

### Listar Todas las Transiciones

```bash
ls -1 transitions/*.glsl | sed 's/.*\///' | sed 's/\.glsl//'
```

## 🎬 Transiciones

| Categoría | Efectos |
|-----------|---------|
| **Geométricas** | Cube, Fold, Flip, Rotate, Spiral, Doorway, Windowblinds |
| **Disolver** | Fade, CrossWarp, Mosaic, Pixelize, CircleCrop |
| **Barrido** | WipeLeft, WipeRight, WipeUp, WipeDown, LinearBlur |
| **Zoom** | ZoomIn, ZoomOut, ZoomInCircles, SimpleZoom |
| **Especiales** | Burn, Glitch, Heart, Jelly, Ripple, WaterDrop |

**Total: 109 transiciones únicas**

Vista previa de todas las transiciones en [gl-transitions.com](https://gl-transitions.com/)

## 📖 Uso

### Parámetros

| Parámetro | Descripción | Predeterminado |
|-----------|-------------|----------------|
| `duration` | Duración de la transición en segundos | 1.0 |
| `source` | Ruta al archivo de sombreador GLSL | fundido integrado |
| `offset` | Desplazamiento de inicio de transición | 0.0 |

### Ejemplos

#### Concatenación de Múltiples Videos

```bash
./bin/ffmpeg -i intro.mp4 -i main.mp4 -i outro.mp4 \
  -filter_complex \
    "[0:v][1:v]gltransition=duration=2[v1]; \
     [v1][2:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 -preset fast -crf 23 final.mp4
```

#### Salida de Alta Calidad

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=3:source=$(pwd)/transitions/crosswarp.glsl" \
  -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k output.mp4
```

#### Codificación Rápida

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=1" \
  -c:v libx264 -preset ultrafast -crf 28 output.mp4
```

## 💻 Requisitos del Sistema

- macOS 12.0 (Monterey) o posterior
- Mac con Apple Silicon (M1/M2/M3/M4/M5)

**Nota:** Los Mac Intel no son compatibles.

## 📦 Estructura del Proyecto

```
ffmpeg-gl-offline-mac/
├── bin/
│   └── ffmpeg              # Binario estático (~22MB)
├── transitions/            # 109 sombreadores GLSL (~440KB)
│   ├── cube.glsl
│   ├── fade.glsl
│   ├── crosswarp.glsl
│   └── ...
├── LICENSE
└── README.md
```

## 🐛 Solución de Problemas

### "shader file not found"

Usa rutas absolutas o relativas a tu directorio actual:

```bash
# ❌ Incorrecto
gltransition=source=cube.glsl

# ✅ Correcto
gltransition=source=$(pwd)/transitions/cube.glsl
gltransition=source=/ruta/completa/a/transitions/cube.glsl
```

### "Sin efecto de transición en la salida"

Asegúrate de que ambos videos de entrada tengan transmisiones de video válidas:

```bash
./bin/ffmpeg -i video1.mp4  # Verificar "Stream #0:0: Video"
```

### Codificación Lenta

Usa preajustes más rápidos:

```bash
-preset fast      # Buen equilibrio
-preset ultrafast # Codificación más rápida
```

## 🤝 Contribuir

¡Las contribuciones son bienvenidas! Siéntete libre de enviar un Pull Request.

1. Haz fork del repositorio
2. Crea tu rama de característica (`git checkout -b feature/AmazingFeature`)
3. Confirma tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Empuja a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está licenciado bajo la **GNU General Public License v2.0 o posterior** debido a la inclusión de:

- FFmpeg (LGPL v2.1+, con componentes GPL v2+)
- libx264 (GPL v2+)

Los sombreadores de transición GLSL en `transitions/` son de [gl-transitions](https://github.com/gl-transitions/gl-transitions) y están licenciados bajo la Licencia MIT.

Consulta [LICENSE](../LICENSE) para más detalles.

## 🙏 Agradecimientos

- [FFmpeg](https://ffmpeg.org/) - El marco multimedia definitivo
- [gl-transitions](https://gl-transitions.com/) - Hermosa biblioteca de transiciones GLSL
- [gl-transition example filter](https://github.com/gl-transitions/gl-transition) - Implementación de referencia

## 📊 Estadísticas del Proyecto

![GitHub stars](https://img.shields.io/github/stars/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub forks](https://img.shields.io/github/forks/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub issues](https://img.shields.io/github/issues/yyjweb/ffmpeg-gl-offline-mac)

---

<div align="center">

**Hecho con ❤️ para la comunidad de edición de video en macOS**

[⬆ Volver Arriba](#ffmpeg-gl-transitions-para-macos)

</div>
