# FFmpeg GL Transitions для macOS

<div align="center">

![FFmpeg](https://img.shields.io/badge/FFmpeg-8.1-green?logo=ffmpeg)
![Платформа](https://img.shields.io/badge/Платформа-Apple%20Silicon-blue?logo=apple)
![Лицензия](https://img.shields.io/badge/Лицензия-GPL%20v2%2B-orange)
![Переходы](https://img.shields.io/badge/Переходы-109-purple)

[🇺🇸 English](../README.md) | [🇨🇳 中文](README_CN.md) | [🇯🇵 日本語](README_JA.md) | [🇰🇷 한국어](README_KO.md) | [🇪🇸 Español](README_ES.md) | [🇫🇷 Français](README_FR.md) | **🇷🇺 Русский** | [🇧🇷 Português](README_PT.md)

**Статический бинарный файл FFmpeg 8.1 со 109 переходами видео GLSL с GPU-ускорением для Apple Silicon (M1/M2/M3/M4/M5)**

[Возможности](#-возможности) • [Быстрый старт](#-быстрый-старт) • [Переходы](#-переходы) • [Использование](#-использование)

</div>

---

## ✨ Возможности

- ✅ **Без зависимостей** - Полная статическая линковка, внешние библиотеки не требуются
- ✅ **Нативный Apple Silicon** - Оптимизировано для M1/M2/M3/M4/M5 (ARM64)
- ✅ **109 переходов** - Полная библиотека gl-transitions включена
- ✅ **Офлайн готовность** - Скачайте и запустите, сеть не требуется
- ✅ **GPU-ускорение** - Использует macOS Core OpenGL для аппаратного рендеринга
- ✅ **FFmpeg 8.1** - Последняя версия FFmpeg со всеми кодеками

## 🚀 Быстрый старт

### Скачать

```bash
# Клонировать репозиторий
git clone https://github.com/yyjweb/ffmpeg-gl-offline-mac.git
cd ffmpeg-gl-offline-mac

# Проверить бинарный файл
./bin/ffmpeg -version
```

### Базовое использование

```bash
# Простой переход затухания (по умолчанию)
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2" \
  -c:v libx264 output.mp4

# Использовать конкретный эффект перехода
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 output.mp4
```

### Список всех переходов

```bash
ls -1 transitions/*.glsl | sed 's/.*\///' | sed 's/\.glsl//'
```

## 🎬 Переходы

| Категория | Эффекты |
|-----------|---------|
| **Геометрические** | Cube, Fold, Flip, Rotate, Spiral, Doorway, Windowblinds |
| **Затухание** | Fade, CrossWarp, Mosaic, Pixelize, CircleCrop |
| **Вытеснение** | WipeLeft, WipeRight, WipeUp, WipeDown, LinearBlur |
| **Масштаб** | ZoomIn, ZoomOut, ZoomInCircles, SimpleZoom |
| **Специальные** | Burn, Glitch, Heart, Jelly, Ripple, WaterDrop |

**Всего: 109 уникальных переходов**

Предпросмотр всех переходов на [gl-transitions.com](https://gl-transitions.com/)

## 📖 Использование

### Параметры

| Параметр | Описание | По умолчанию |
|----------|----------|--------------|
| `duration` | Длительность перехода в секундах | 1.0 |
| `source` | Путь к файлу шейдера GLSL | встроенный fade |
| `offset` | Смещение начала перехода | 0.0 |

### Примеры

#### Объединение нескольких видео

```bash
./bin/ffmpeg -i intro.mp4 -i main.mp4 -i outro.mp4 \
  -filter_complex \
    "[0:v][1:v]gltransition=duration=2[v1]; \
     [v1][2:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 -preset fast -crf 23 final.mp4
```

#### Высококачественный вывод

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=3:source=$(pwd)/transitions/crosswarp.glsl" \
  -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k output.mp4
```

#### Быстрое кодирование

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=1" \
  -c:v libx264 -preset ultrafast -crf 28 output.mp4
```

## 💻 Системные требования

- macOS 12.0 (Monterey) или новее
- Mac с Apple Silicon (M1/M2/M3/M4/M5)

**Примечание:** Mac на Intel не поддерживаются.

## 📦 Структура проекта

```
ffmpeg-gl-offline-mac/
├── bin/
│   └── ffmpeg              # Статический бинарный файл (~22MB)
├── transitions/            # 109 шейдеров GLSL (~440KB)
│   ├── cube.glsl
│   ├── fade.glsl
│   ├── crosswarp.glsl
│   └── ...
├── LICENSE
└── README.md
```

## 🐛 Устранение неполадок

### "shader file not found"

Используйте абсолютные пути или пути относительно текущей директории:

```bash
# ❌ Неправильно
gltransition=source=cube.glsl

# ✅ Правильно
gltransition=source=$(pwd)/transitions/cube.glsl
gltransition=source=/полный/путь/к/transitions/cube.glsl
```

### "Нет эффекта перехода в выводе"

Убедитесь, что оба входных видео имеют действительные видеопотоки:

```bash
./bin/ffmpeg -i video1.mp4  # Проверить "Stream #0:0: Video"
```

### Медленное кодирование

Используйте более быстрые пресеты:

```bash
-preset fast      # Хороший баланс
-preset ultrafast # Самое быстрое кодирование
```

## 🤝 Участие в разработке

Вклад приветствуется! Не стесняйтесь отправлять Pull Request.

1. Сделайте форк репозитория
2. Создайте ветку функции (`git checkout -b feature/AmazingFeature`)
3. Зафиксируйте изменения (`git commit -m 'Add some AmazingFeature'`)
4. Отправьте в ветку (`git push origin feature/AmazingFeature`)
5. Откройте Pull Request

## 📄 Лицензия

Этот проект лицензирован под **GNU General Public License v2.0 или позднее** из-за включения:

- FFmpeg (LGPL v2.1+, с компонентами GPL v2+)
- libx264 (GPL v2+)

Шейдеры переходов GLSL в `transitions/` из [gl-transitions](https://github.com/gl-transitions/gl-transitions) и лицензированы под MIT License.

См. [LICENSE](../LICENSE) для подробностей.

## 🙏 Благодарности

- [FFmpeg](https://ffmpeg.org/) - Универсальный мультимедийный фреймворк
- [gl-transitions](https://gl-transitions.com/) - Красивая библиотека переходов GLSL
- [gl-transition example filter](https://github.com/gl-transitions/gl-transition) - Эталонная реализация

## 📊 Статистика проекта

![GitHub stars](https://img.shields.io/github/stars/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub forks](https://img.shields.io/github/forks/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub issues](https://img.shields.io/github/issues/yyjweb/ffmpeg-gl-offline-mac)

---

<div align="center">

**Сделано с ❤️ для сообщества видеомонтажа macOS**

[⬆ Наверх](#ffmpeg-gl-transitions-для-macos)

</div>
