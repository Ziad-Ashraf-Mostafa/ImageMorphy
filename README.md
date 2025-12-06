<div align="center">
  
# 🎭 Morphy - Real-Time Face Morphing App

</div>

Transform your face in real-time video into animals, historical figures, and celebrities using cutting-edge AR technology. Morphy is a cross-platform mobile application built with Flutter and powered by DeepAR's advanced 3D face morphing capabilities.

## ✨ Features

- **Real-Time Face Morphing** – Smooth, physically-accurate 3D face deformation (not just 2D stickers)
- **Dynamic Filter Loading** – Add new morphs without releasing app store updates
- **Cross-Platform Support** – Available on iOS, Android, Windows, macOS, Linux, and Web
- **Extensible Architecture** – Easy-to-add new filters and effects

## 🏗️ Project Overview

Morphy uses **Blendshape technology** to interpolate between your real face and target morphs (animals, celebrities, historical figures). The app dynamically fetches filter metadata and 3D assets from GitHub Pages, enabling rapid iteration without app store bottlenecks.

### Why DeepAR?

We evaluated multiple AR engines:
- **MediaPipe** – Too low-level; requires extensive custom implementation
- **Snap Camera Kit** – Too restrictive for our use case
- **DeepAR** ✅ – Perfect balance of power, ease-of-use, and customization

---

## 📋 Tech Stack

| Component | Technology |
|-----------|-----------|
| **Framework** | [Flutter](https://flutter.dev/) (Dart) |
| **AR Engine** | [DeepAR](https://www.deepar.ai/) (`deepar_flutter_plus` package) |
| **Asset Hosting** | GitHub Pages |
| **Version Control** | Git with LFS for 3D assets |
| **3D Tools** | FaceBuilder for Blender, DeepAR Studio |

---

## 🚀 Quick Start

### Prerequisites

Before you begin, ensure you have the following installed:

- **Flutter SDK** (3.x or higher) – [Install Guide](https://flutter.dev/docs/get-started/install)
- **Git** and **Git LFS** – [Git LFS Setup](https://git-lfs.github.com/)
- **Xcode** (macOS/iOS development) or **Android Studio** (Android development)
- **Dart SDK** (included with Flutter)

### Step 1: Clone the Repository

```bash
# Clone the repo with LFS support
git clone https://github.com/AhmadEnan/Morphy.git
cd Morphy

# Ensure Git LFS is initialized
git lfs install
git lfs pull
```

### Step 2: Install Dependencies

```bash
# Get all Flutter dependencies
flutter pub get

# (Optional) For iOS development, install pods
cd ios
pod install
cd ..
```

### Step 3: Run the App

```bash
# List available devices
flutter devices

# Run on your device/emulator
flutter run

# Or specify a device
flutter run -d <device_id>
```

### Step 4: Verify Setup

1. Launch the app
2. Grant camera permissions
3. Tap a morph filter to test real-time morphing

---

## 📁 Project Structure

```
lib/
├── main.dart                 # App entry point
├── ar/                       # DeepAR integration & camera logic
│   ├── deepar_controller.dart
│   ├── camera_manager.dart
│   └── effects_renderer.dart
├── ui/                       # UI components & screens
│   ├── screens/
│   │   ├── camera_screen.dart
│   │   ├── effects_gallery.dart
│   │   └── settings_screen.dart
│   ├── widgets/
│   │   ├── morph_button.dart
│   │   ├── slider_control.dart
│   │   └── loading_indicator.dart
│   └── theme/
│       ├── app_theme.dart
│       └── colors.dart
├── services/                 # Backend & data fetching
│   ├── effect_service.dart   # Fetches effects.json from GitHub Pages
│   ├── asset_downloader.dart # Downloads .deepar files
│   └── storage_manager.dart  # Local caching
└── models/                   # Data models
    ├── effect.dart
    ├── morph_target.dart
    └── api_response.dart

web_assets/                   # External folder (not in repo)
├── effects.json              # Filter metadata & URLs
└── filters/
    ├── einstein.deepar
    ├── lion.deepar
    └── cleopatra.deepar
```

### Directory Guide by Role

| Directory | Owner | Responsibility |
|-----------|-------|-----------------|
| `lib/ui/` | UI/UX Team | Buttons, layouts, sliders, themes |
| `lib/ar/` | AR Integration Team | DeepAR controller, camera setup |
| `lib/services/` | Backend Team | JSON fetching, asset management, caching |
| `web_assets/` | Filter Creator | `.deepar` files, `effects.json` updates |

---

## 🎨 How to Add a New Morph Filter

### For Filter Creators

1. **Create the 3D Mesh**
   - Take a 2D reference photo of your target (historical figure, animal, etc.)
   - Use FaceBuilder for Blender to wrap a clean topology mesh onto the photo
   - Export the mesh as compatible format for DeepAR Studio

2. **Build in DeepAR Studio**
   - Import the head mesh as Blendshapes
   - Set up morphing parameters (min: 0.0, max: 1.0)
   - Test the deformation in preview mode
   - Export as `.deepar` file

3. **Upload Assets**
   - Push `.deepar` file to `web_assets/filters/`
   - Update `web_assets/effects.json`:
     ```json
     {
       "id": "einstein",
       "name": "Einstein",
       "icon": "https://raw.githubusercontent.com/AhmadEnan/Morphy/main/web_assets/filters/icons/einstein.png",
       "file": "https://raw.githubusercontent.com/AhmadEnan/Morphy/main/web_assets/filters/einstein.deepar",
       "category": "historical"
     }
     ```

4. **Deploy**
   - Commit changes to `web_assets/`
   - Push to `main` branch (GitHub Pages auto-deploys)
   - Test in-app by fetching the new filter

### For Backend Developers

The app fetches filter metadata from:
```
https://raw.githubusercontent.com/AhmadEnan/Morphy/main/web_assets/effects.json
```

When filters are added, the app automatically:
1. Downloads the latest `effects.json`
2. Generates UI buttons dynamically
3. Caches filters locally for offline use

---

## 📚 Architecture Deep Dive

### Dynamic Filter Loading Pipeline

```
App Launch
    ↓
Fetch effects.json from GitHub Pages
    ↓
Parse filter metadata
    ↓
Generate UI buttons
    ↓
User selects filter
    ↓
Download .deepar file (or load from cache)
    ↓
Load into DeepAR via blendshape interpolation (0.0 → 1.0)
    ↓
Real-time face morphing ✨
```

### Blendshape Morphing

Morphing is achieved by interpolating between:
- **0.0** = User's real face
- **0.5** = 50% blend
- **1.0** = Full target morph

This smooth interpolation creates the realistic deformation effect.

---

## 🤝 Contributing

We welcome contributions! Here's how to get started:

1. **Fork the repository** – Create your own copy on GitHub
2. **Create a feature branch** – `git checkout -b feature/amazing-feature`
3. **Make your changes** – Follow the structure guidelines above
4. **Test thoroughly** – Run `flutter test` before pushing
5. **Commit with clear messages** – Use descriptive commit messages
6. **Push and create a Pull Request** – Request review from relevant team leads

### Code Style

- Follow [Dart Style Guide](https://dart.dev/guides/language/effective-dart/style)
- Use meaningful variable names
- Keep functions focused and small
- Add comments for complex logic
- Format code: `dart format .`

### Pull Request Checklist

- [ ] Code follows project style guidelines
- [ ] Tests pass: `flutter test`
- [ ] No new warnings from `flutter analyze`
- [ ] PR description explains the change
- [ ] Related issues are linked

---

## 📖 Resources

### Flutter & Dart
- [Flutter Official Docs](https://flutter.dev/docs)
- [Dart Language Tour](https://dart.dev/guides/language/language-tour)

### DeepAR
- [DeepAR Documentation](https://help.deepar.ai/)
- [DeepAR Studio Guide](https://help.deepar.ai/en/articles/8421048-deepar-studio)

### 3D Asset Creation
- [FaceBuilder for Blender](https://github.com/keentools/keentools-blender)
- [Blender Documentation](https://docs.blender.org/)

---

## 📄 License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

---

## 🎉 Acknowledgments

- **DeepAR** for powerful AR capabilities
- **Flutter** community for excellent documentation
- **Contributors** who help make Morphy better

---

**Happy morphing! 🎭✨**