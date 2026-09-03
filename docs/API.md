# 📚 API Reference - Jarvis AI Assistant

## Overview

Jarvis AI exposes APIs for developers to create custom integrations, plugins, and extensions.

## Authentication

All API calls require an API key:

```kotlin
JarvisAPI.setApiKey("your-api-key")
```

## Core APIs

### 1. Voice Recognition API

```kotlin
JarvisAPI.startVoiceRecognition(
    language = "en-US",
    timeout = 10000,
    onResult = { text ->
        Log.d("Jarvis", "Recognized: $text")
    },
    onError = { error ->
        Log.e("Jarvis", "Error: $error")
    }
)
```

### 2. Text-to-Speech API

```kotlin
JarvisAPI.speak(
    text = "Hello, I'm Jarvis",
    language = "en-US",
    speed = 1.0f,
    pitch = 1.0f,
    onComplete = {
        Log.d("Jarvis", "Speech completed")
    }
)
```

### 3. Command Execution API

```kotlin
JarvisAPI.executeCommand(
    command = "Open Instagram",
    onSuccess = { result ->
        Log.d("Jarvis", "Command executed: $result")
    },
    onError = { error ->
        Log.e("Jarvis", "Command failed: $error")
    }
)
```

### 4. AI Processing API

```kotlin
JarvisAPI.processWithAI(
    input = "What's the weather?",
    model = AIModel.OFFLINE, // or CLOUD, HYBRID
    onResult = { response ->
        Log.d("Jarvis", "AI Response: $response")
    }
)
```

## Data Models

### Command Model

```kotlin
data class Command(
    val id: String,
    val name: String,
    val description: String,
    val action: String,
    val category: CommandCategory,
    val requiredPermissions: List<String>,
    val enabled: Boolean
)

enum class CommandCategory {
    DEVICE_CONTROL,
    COMMUNICATION,
    INFORMATION,
    MEDIA,
    SMART_HOME,
    CUSTOM
}
```

### Voice Recognition Result

```kotlin
data class VoiceResult(
    val text: String,
    val confidence: Float, // 0.0 to 1.0
    val language: String,
    val timestamp: Long,
    val isFinal: Boolean
)
```

### AI Response

```kotlin
data class AIResponse(
    val text: String,
    val intent: String,
    val entities: List<Entity>,
    val confidence: Float,
    val source: String // "offline" or "cloud"
)

data class Entity(
    val type: String,
    val value: String,
    val confidence: Float
)
```

## Event Listeners

### Listen to Voice Events

```kotlin
JarvisAPI.addVoiceListener { event ->
    when (event) {
        is VoiceEvent.Started -> Log.d("Jarvis", "Listening...")
        is VoiceEvent.Processing -> Log.d("Jarvis", "Processing...")
        is VoiceEvent.Recognized -> Log.d("Jarvis", "Heard: ${event.text}")
        is VoiceEvent.Error -> Log.e("Jarvis", "Error: ${event.error}")
    }
}
```

### Listen to Command Execution

```kotlin
JarvisAPI.addCommandListener { event ->
    when (event) {
        is CommandEvent.Started -> Log.d("Jarvis", "Executing command")
        is CommandEvent.Progress -> Log.d("Jarvis", "${event.progress}%")
        is CommandEvent.Completed -> Log.d("Jarvis", "Done")
        is CommandEvent.Failed -> Log.e("Jarvis", "Failed: ${event.error}")
    }
}
```

## REST API Endpoints

### Authentication

```
POST /api/auth/login
Body: { "username": "...", "password": "..." }
Response: { "token": "...", "expires_in": 3600 }
```

### Voice Recognition

```
POST /api/voice/recognize
Headers: Authorization: Bearer {token}
Body: { "audio": "base64_encoded_audio", "language": "en-US" }
Response: { "text": "...", "confidence": 0.95 }
```

### Text-to-Speech

```
POST /api/voice/synthesize
Headers: Authorization: Bearer {token}
Body: { "text": "Hello", "language": "en-US", "voice": "female" }
Response: { "audio": "base64_encoded_audio", "duration": 1500 }
```

### Command Execution

```
POST /api/commands/execute
Headers: Authorization: Bearer {token}
Body: { "command": "Open Instagram", "params": {} }
Response: { "status": "success", "result": "...", "timestamp": 1234567890 }
```

### Get Commands

```
GET /api/commands?category=DEVICE_CONTROL&enabled=true
Headers: Authorization: Bearer {token}
Response: { "commands": [...], "total": 50 }
```

## Error Handling

```kotlin
try {
    val result = JarvisAPI.executeCommand("Open Instagram")
} catch (e: JarvisException) {
    when (e) {
        is PermissionDeniedException -> {
            // Request permission
        }
        is CommandNotFoundException -> {
            // Command doesn't exist
        }
        is InternetRequiredException -> {
            // Switch to offline mode or show error
        }
        else -> {
            // Handle other errors
        }
    }
}
```

## Advanced Features

### Custom Command Registration

```kotlin
val customCommand = Command(
    id = "custom_1",
    name = "Make Coffee",
    description = "Turn on coffee maker",
    action = "turnOnDevice:coffee_maker",
    category = CommandCategory.CUSTOM
)

JarvisAPI.registerCommand(customCommand)
```

### Machine Learning Integration

```kotlin
JarvisAPI.trainModel(
    trainingData = listOf(...),
    modelType = ModelType.VOICE_RECOGNITION,
    onProgress = { progress ->
        Log.d("Jarvis", "Training: $progress%")
    },
    onComplete = {
        Log.d("Jarvis", "Training complete")
    }
)
```

### Plugin System

```kotlin
interface JarvisPlugin {
    fun onLoad()
    fun onUnload()
    fun getCommands(): List<Command>
    fun execute(command: Command): Result
}

class CustomPlugin : JarvisPlugin {
    override fun onLoad() {
        // Initialize plugin
    }
    
    override fun getCommands(): List<Command> {
        return listOf(...)
    }
}

JarvisAPI.installPlugin(CustomPlugin())
```

## Rate Limiting

- **Free Tier**: 100 requests/minute
- **Pro Tier**: 1000 requests/minute
- **Enterprise**: Unlimited

## Changelog

### v1.0.0
- Initial API release
- Voice recognition & synthesis
- Command execution
- Basic AI integration

## Support

- Documentation: [Developer Docs](https://jarvisai.dev/docs)
- Issues: [GitHub Issues](https://github.com/Abhisekhgajjangi4321/JarvisAI-Assistant/issues)
- Discord: [Community Server](https://discord.gg/jarvisai)
