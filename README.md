# VibeTranscribe

A Mendix module that provides offline speech-to-text transcription capabilities using the Vosk speech recognition library with dynamic model loading.

## Overview

VibeTranscribe enables your Mendix application to transcribe audio files to text without requiring external services or internet connectivity. It uses the Vosk library with user-uploaded language models to deliver accurate speech recognition locally within your Mendix environment. The module supports multiple languages through dynamically loaded Vosk models.

## Features

- 🎯 **Offline Transcription**: No internet connection required
- 🚀 **Fast Processing**: Intelligent caching for optimal performance
- 📱 **Multiple Format Support**: Handles various audio formats (WAV recommended)
- 🔧 **Easy Integration**: Simple Java action for seamless Mendix integration
- 📊 **Detailed Logging**: Comprehensive logging for debugging and monitoring
- 🌐 **Multi-Language Support**: Works with any Vosk language model
- ⚡ **Smart Caching**: Efficient model caching with SHA-256 content hashing
- 🔒 **Secure Model Loading**: Safe ZIP extraction with security validation

## Implementation

To implement VibeTranscribe in your Mendix project:

### 1. Import the Module

1. Download the latest **VibeTranscribe.mpk** file
2. In Mendix Studio Pro, go to **File** > **Import Module Package...**
3. Select the downloaded MPK file
4. Follow the import wizard to complete the installation

### 2. Module Contents

After importing, the module provides:

- **Java Action**: `TranscribeAudioFile` - Main transcription functionality
- **Required Libraries** (automatically managed by Mendix/Gradle): 
  - Vosk (0.3.45) - Speech recognition library (`com.alphacephei:vosk`)
  - Jackson libraries (2.19.2) - JSON processing (`com.fasterxml.jackson.core`)
  - JNA (5.17.0) - Java Native Access (`net.java.dev.jna:jna`)

### 3. Vosk Model Setup

**Download a Vosk Model:**
1. Visit [Vosk Models](https://alphacephei.com/vosk/models) and download your preferred language model
2. Models are available for many languages (English, Spanish, German, French, Russian, etc.)
3. Choose between small (~50MB) or large (~1-2GB) models based on your accuracy needs

**Recommended Models:**
- **English**: `vosk-model-small-en-us-0.15` (40MB) or `vosk-model-en-us-0.22` (1.8GB)
- **Multi-language**: `vosk-model-small-en-us-0.15` for general use
- **Other languages**: Check the Vosk website for language-specific models

### 4. Usage

#### Basic Implementation

1. **Upload a Vosk Model** to your Mendix application as a FileDocument (ZIP format)
2. **Create a Microflow** in your domain model
3. **Add the TranscribeAudioFile Java Action** to your microflow
4. **Pass two parameters**:
   - AudioFile: FileDocument containing your audio file
   - VoskModel: FileDocument containing the downloaded Vosk model (ZIP format)
5. **Capture the return value** (String) which contains the transcribed text

#### Example Microflow Steps

```
1. [Start] → 
2. [Retrieve Audio FileDocument] → 
3. [Retrieve Vosk Model FileDocument] → 
4. [Call TranscribeAudioFile(AudioFile, VoskModel)] → 
5. [Process/Store Transcription Result] → 
6. [End]
```

#### Input Requirements

- **AudioFile Parameter**: FileDocument entity with audio content
  - **Supported Formats**: WAV (recommended), MP3, M4A, and other common audio formats
  - **Optimal Settings**: 16kHz, 16-bit, mono WAV files for best performance

- **VoskModel Parameter**: FileDocument entity with Vosk model ZIP file
  - **Format**: ZIP file containing extracted Vosk model directory
  - **Source**: Downloaded from [Vosk Models](https://alphacephei.com/vosk/models)
  - **Languages**: Any language supported by Vosk (English, Spanish, German, etc.)

#### Return Value

- **Type**: String
- **Content**: Transcribed text from the audio file
- **Empty Result**: Returns informative message if no speech is detected

## Technical Details

### Dependencies

This module relies on the following Maven dependencies that are automatically managed by Mendix:

```xml
<!-- Vosk Speech Recognition -->
<dependency>
    <groupId>com.alphacephei</groupId>
    <artifactId>vosk</artifactId>
    <version>0.3.45</version>
</dependency>

<!-- Jackson JSON Processing -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.19.2</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.19.2</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.19.2</version>
</dependency>

<!-- Java Native Access -->
<dependency>
    <groupId>net.java.dev.jna</groupId>
    <artifactId>jna</artifactId>
    <version>5.17.0</version>
</dependency>
```

### Audio Processing

The module automatically:
- Validates audio file formats
- Converts audio to compatible format when necessary (16kHz, mono)
- Processes audio in optimized chunks
- Handles WAV headers appropriately

### Model Caching

The module implements intelligent caching to optimize performance:
- **Content-Based Hashing**: Uses SHA-256 to identify unique model content
- **Automatic Reuse**: Same model content is cached across requests
- **Memory Efficient**: Models are extracted only once per unique content
- **Automatic Cleanup**: Old cached models are cleaned up after 7 days
- **Performance Gain**: Subsequent requests with the same model are 10-30x faster

### Performance

- **Model Size**: Varies by language (40MB - 2GB depending on chosen model)
- **Processing Speed**: Real-time or faster depending on audio length
- **Memory Usage**: Optimized footprint suitable for production environments
- **Caching Benefits**: 
  - First request: 5-15 seconds (includes model extraction)
  - Cached requests: 0.5-2 seconds (model already loaded)

### Error Handling

The module provides comprehensive error handling for:
- Invalid or corrupted audio files
- Missing or invalid Vosk model files
- Malformed ZIP model packages
- Insufficient system resources
- Unsupported audio formats
- Model extraction and caching failures

## Requirements

### Mendix Platform
- **Mendix Version**: 10.24.2 or higher
- **Java Version**: Java 11 or higher

### System Requirements
- **RAM**: Minimum 1GB available memory (more for larger models)
- **Storage**: 200MB-5GB depending on chosen Vosk model size
- **Platform**: Windows, Linux, macOS (wherever Mendix runs)
- **Temporary Space**: Additional space for model caching (cleaned automatically)

## Deployment

### Local Development
1. Import the MPK file into your project
2. Download a Vosk model from [alphacephei.com/vosk/models](https://alphacephei.com/vosk/models)
3. Upload the model ZIP file through your application
4. Test transcription functionality

### Production Deployment

1. **Model Management**: Plan for model storage and upload strategy
2. **Cache Location**: Ensure temp directory has sufficient space for model caching
3. **Performance**: Consider using smaller models for faster deployment
4. **Security**: Validate uploaded model files before processing

### Cloud Deployment

For Mendix Cloud deployments:
1. All required JAR files are automatically included via Maven
2. Models are uploaded dynamically through your application
3. Caching works automatically in cloud environments
4. Monitor disk space for temporary model files

## Troubleshooting

### Common Issues

**"No Vosk model provided" Error**
- Ensure you pass a valid Vosk model ZIP file as the second parameter
- Verify the model file has content (FileDocument.getHasContents() returns true)
- Download models from the official Vosk website

**"Could not find valid Vosk model files" Error**
- Verify the ZIP file contains a properly extracted Vosk model
- Check that required directories (am/, graph/, conf/) exist in the ZIP
- Re-download the model if it appears corrupted

**"No speech detected" Result**
- Ensure audio file contains clear speech
- Verify audio format compatibility (WAV recommended)
- Check audio quality and volume levels
- Verify the language model matches the spoken language

**Performance Issues**
- Use smaller Vosk models for faster processing
- WAV format provides optimal processing speed
- Monitor cache hit/miss ratios in logs
- Ensure sufficient system memory for chosen model size

**Cache Issues**
- Check temporary directory permissions
- Monitor available disk space
- Review cache cleanup logs (runs every 24 hours)

### Logging

Enable detailed logging by setting the `VibeTranscribe` log level to `DEBUG` in your Mendix application settings. This provides information about:
- Model extraction and caching
- Audio format analysis and conversion
- Performance metrics and timing
- Cache hit/miss statistics
- Error details and troubleshooting information

## License

This module includes the following third-party components:
- **Vosk**: Apache License 2.0
- **Jackson**: Apache License 2.0
- **JNA**: LGPL 2.1 / Apache License 2.0

## Support

For questions, issues, or feature requests, please refer to the module documentation or contact the development team.

## Version History

- **1.0.0**: Initial release with dynamic model loading
  - Dynamic Vosk model loading from user-uploaded ZIP files
  - Intelligent caching with SHA-256 content hashing
  - Multi-language support through any Vosk model
  - Updated dependencies: Jackson 2.19.2, JNA 5.17.0, Vosk 0.3.45
  - Automatic audio format conversion (to 16kHz mono)
  - Enhanced security with ZIP extraction validation
  - Optimized for Mendix 10.24.2+ environments
  - Production-ready caching and cleanup mechanisms