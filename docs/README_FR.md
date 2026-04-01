# FFmpeg GL Transitions pour macOS

<div align="center">

![FFmpeg](https://img.shields.io/badge/FFmpeg-8.1-green?logo=ffmpeg)
![Plateforme](https://img.shields.io/badge/Plateforme-Apple%20Silicon-blue?logo=apple)
![Licence](https://img.shields.io/badge/Licence-GPL%20v2%2B-orange)
![Transitions](https://img.shields.io/badge/Transitions-109-purple)

[🇺🇸 English](../README.md) | [🇨🇳 中文](README_CN.md) | [🇯🇵 日本語](README_JA.md) | [🇰🇷 한국어](README_KO.md) | [🇪🇸 Español](README_ES.md) | **🇫🇷 Français** | [🇷🇺 Русский](README_RU.md) | [🇧🇷 Português](README_PT.md)

**Binaire statique FFmpeg 8.1 avec 109 transitions vidéo GLSL accélérées par GPU pour Apple Silicon (M1/M2/M3/M4/M5)**

[Fonctionnalités](#-fonctionnalités) • [Démarrage Rapide](#-démarrage-rapide) • [Transitions](#-transitions) • [Utilisation](#-utilisation)

</div>

---

## ✨ Fonctionnalités

- ✅ **Zéro Dépendance** - Liaison statique complète, aucune bibliothèque externe requise
- ✅ **Natif Apple Silicon** - Optimisé pour M1/M2/M3/M4/M5 (ARM64)
- ✅ **109 Transitions** - Bibliothèque complète gl-transitions incluse
- ✅ **Prêt Hors Ligne** - Téléchargez et exécutez, aucun réseau requis
- ✅ **Accélération GPU** - Utilise macOS Core OpenGL pour le rendu matériel
- ✅ **FFmpeg 8.1** - Dernière version de FFmpeg avec tous les codecs

## 🚀 Démarrage Rapide

### Télécharger

```bash
# Cloner le dépôt
git clone https://github.com/yyjweb/ffmpeg-gl-offline-mac.git
cd ffmpeg-gl-offline-mac

# Vérifier le binaire
./bin/ffmpeg -version
```

### Utilisation de Base

```bash
# Transition de fondu simple (par défaut)
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2" \
  -c:v libx264 output.mp4

# Utiliser un effet de transition spécifique
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 output.mp4
```

### Lister Toutes les Transitions

```bash
ls -1 transitions/*.glsl | sed 's/.*\///' | sed 's/\.glsl//'
```

## 🎬 Transitions

| Catégorie | Effets |
|-----------|--------|
| **Géométriques** | Cube, Fold, Flip, Rotate, Spiral, Doorway, Windowblinds |
| **Fondu** | Fade, CrossWarp, Mosaic, Pixelize, CircleCrop |
| **Balayage** | WipeLeft, WipeRight, WipeUp, WipeDown, LinearBlur |
| **Zoom** | ZoomIn, ZoomOut, ZoomInCircles, SimpleZoom |
| **Spéciales** | Burn, Glitch, Heart, Jelly, Ripple, WaterDrop |

**Total : 109 transitions uniques**

Prévisualisez toutes les transitions sur [gl-transitions.com](https://gl-transitions.com/)

## 📖 Utilisation

### Paramètres

| Paramètre | Description | Défaut |
|-----------|-------------|--------|
| `duration` | Durée de la transition en secondes | 1.0 |
| `source` | Chemin vers le fichier shader GLSL | fondu intégré |
| `offset` | Décalage de début de transition | 0.0 |

### Exemples

#### Concaténation de Plusieurs Vidéos

```bash
./bin/ffmpeg -i intro.mp4 -i main.mp4 -i outro.mp4 \
  -filter_complex \
    "[0:v][1:v]gltransition=duration=2[v1]; \
     [v1][2:v]gltransition=duration=2:source=$(pwd)/transitions/cube.glsl" \
  -c:v libx264 -preset fast -crf 23 final.mp4
```

#### Sortie Haute Qualité

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=3:source=$(pwd)/transitions/crosswarp.glsl" \
  -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k output.mp4
```

#### Encodage Rapide

```bash
./bin/ffmpeg -i video1.mp4 -i video2.mp4 \
  -filter_complex "[0:v][1:v]gltransition=duration=1" \
  -c:v libx264 -preset ultrafast -crf 28 output.mp4
```

## 💻 Configuration Requise

- macOS 12.0 (Monterey) ou version ultérieure
- Mac Apple Silicon (M1/M2/M3/M4/M5)

**Remarque :** Les Mac Intel ne sont pas pris en charge.

## 📦 Structure du Projet

```
ffmpeg-gl-offline-mac/
├── bin/
│   └── ffmpeg              # Binaire statique (~22MB)
├── transitions/            # 109 shaders GLSL (~440KB)
│   ├── cube.glsl
│   ├── fade.glsl
│   ├── crosswarp.glsl
│   └── ...
├── LICENSE
└── README.md
```

## 🐛 Dépannage

### "shader file not found"

Utilisez des chemins absolus ou relatifs à votre répertoire actuel :

```bash
# ❌ Incorrect
gltransition=source=cube.glsl

# ✅ Correct
gltransition=source=$(pwd)/transitions/cube.glsl
gltransition=source=/chemin/complet/vers/transitions/cube.glsl
```

### "Pas d'effet de transition dans la sortie"

Assurez-vous que les deux vidéos d'entrée ont des flux vidéo valides :

```bash
./bin/ffmpeg -i video1.mp4  # Vérifier "Stream #0:0: Video"
```

### Encodage Lent

Utilisez des préréglages plus rapides :

```bash
-preset fast      # Bon équilibre
-preset ultrafast # Encodage le plus rapide
```

## 🤝 Contribuer

Les contributions sont les bienvenues ! N'hésitez pas à soumettre une Pull Request.

1. Forkez le dépôt
2. Créez votre branche de fonctionnalité (`git checkout -b feature/AmazingFeature`)
3. Validez vos changements (`git commit -m 'Add some AmazingFeature'`)
4. Poussez vers la branche (`git push origin feature/AmazingFeature`)
5. Ouvrez une Pull Request

## 📄 Licence

Ce projet est sous licence **GNU General Public License v2.0 ou ultérieure** en raison de l'inclusion de :

- FFmpeg (LGPL v2.1+, avec des composants GPL v2+)
- libx264 (GPL v2+)

Les shaders de transition GLSL dans `transitions/` proviennent de [gl-transitions](https://github.com/gl-transitions/gl-transitions) et sont sous licence MIT.

Consultez [LICENSE](../LICENSE) pour plus de détails.

## 🙏 Remerciements

- [FFmpeg](https://ffmpeg.org/) - Le cadre multimédia ultime
- [gl-transitions](https://gl-transitions.com/) - Belle bibliothèque de transitions GLSL
- [gl-transition example filter](https://github.com/gl-transitions/gl-transition) - Implémentation de référence

## 📊 Statistiques du Projet

![GitHub stars](https://img.shields.io/github/stars/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub forks](https://img.shields.io/github/forks/yyjweb/ffmpeg-gl-offline-mac?style=social)
![GitHub issues](https://img.shields.io/github/issues/yyjweb/ffmpeg-gl-offline-mac)

---

<div align="center">

**Fait avec ❤️ pour la communauté d'édition vidéo macOS**

[⬆ Retour en Haut](#ffmpeg-gl-transitions-pour-macos)

</div>
