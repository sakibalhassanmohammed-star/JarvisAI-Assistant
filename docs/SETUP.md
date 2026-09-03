# 🚀 Setup Guide - Jarvis AI Assistant

## Prerequisites

### System Requirements
- **Android Studio** 4.2+ or higher
- **JDK 11+**
- **Gradle 7.0+**
- **Min SDK**: 26 (Android 8.0)
- **Target SDK**: 34 (Android 14)

### Device Requirements
- Android 8.0 or higher
- Minimum 2GB RAM
- 150MB free storage
- Microphone access

## Installation Steps

### Step 1: Clone Repository
```bash
git clone https://github.com/Abhisekhgajjangi4321/JarvisAI-Assistant.git
cd JarvisAI-Assistant
```

### Step 2: Open in Android Studio
1. Open Android Studio
2. Click "Open an Existing Project"
3. Navigate to the cloned directory
4. Click "Open"
5. Wait for Gradle sync to complete

### Step 3: Configure API Keys

Create `local.properties` file in project root:
```properties
sdk.dir=/path/to/android/sdk
GOOGLE_API_KEY=your_google_api_key
OPENAI_API_KEY=your_openai_api_key
```

### Step 4: Build the Project
```bash
# Build APK
./gradlew assembleDebug

# Build Release APK
./gradlew assembleRelease
```

### Step 5: Install on Device

**Option A: Direct Installation**
```bash
./gradlew installDebug
```

**Option B: Manual Installation**
1. Connect device via USB
2. Enable Developer Mode (tap Build Number 7 times)
3. Enable USB Debugging
4. Run: `adb install app/build/outputs/apk/debug/app-debug.apk`

### Step 6: Grant Permissions

On first launch, grant these permissions:
- ✅ Microphone
- ✅ Camera
- ✅ Contacts
- ✅ Call Log
- ✅ SMS
- ✅ Location
- ✅ Storage

## Configuration

### Voice Recognition Setup
1. Open Jarvis app
2. Go to Settings → Voice
3. Choose language (English, Spanish, Hindi, etc.)
4. Set microphone sensitivity
5. Configure wake word (default: "Jarvis")

### AI Model Selection
1. Settings → AI Model
2. Choose between:
   - **Offline**: TensorFlow Lite (no internet needed)
   - **Cloud**: Google Cloud NLP (better accuracy)
   - **Hybrid**: Uses offline, falls back to cloud

### API Configuration

**Google API Setup:**
1. Visit [Google Cloud Console](https://console.cloud.google.com)
2. Create new project
3. Enable these APIs:
   - Speech-to-Text API
   - Text-to-Speech API
   - Google Search API
4. Create API key
5. Add to `local.properties`

**OpenAI Setup (Optional):**
1. Visit [OpenAI Platform](https://platform.openai.com)
2. Create API key
3. Add to app settings

## Troubleshooting

### Build Issues

**Problem**: Gradle sync fails
```bash
# Solution
./gradlew clean
./gradlew build --refresh-dependencies
```

**Problem**: API key errors
- Check `local.properties` file
- Verify API key is valid
- Ensure APIs are enabled in console

### Runtime Issues

**Microphone not working**
- Check microphone permissions
- Test with other apps
- Restart device

**Voice recognition fails**
- Enable internet connection
- Check microphone quality
- Test with clear voice commands

**App crashes**
- Check logcat: `adb logcat`
- Check minimum Android version
- Ensure sufficient RAM

## Development Setup

### IDE Configuration

**Android Studio Plugins**:
- Install Kotlin Compiler
- Install Android Emulator
- Install Firebase Tools (optional)

### Database Setup

The app uses Room database (local SQLite):
```kotlin
// Database auto-initializes on first launch
// No manual setup required
```

### Testing

```bash
# Run unit tests
./gradlew test

# Run instrumented tests (on device/emulator)
./gradlew connectedAndroidTest
```

## Next Steps

1. Read [COMMANDS.md](COMMANDS.md) for available voice commands
2. Check [API.md](API.md) for developer documentation
3. Explore Settings for customization
4. Enable cloud features in Settings

## Support

- GitHub Issues: [Open Issue](https://github.com/Abhisekhgajjangi4321/JarvisAI-Assistant/issues)
- Documentation: Check docs folder
- Discord: Join community server
