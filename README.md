# VibeTranscribe

A Mendix module that provides offline speech-to-text transcription capabilities using the Vosk speech recognition library.

## Overview

VibeTranscribe enables your Mendix application to transcribe audio files to text without requiring external services or internet connectivity. It uses the Vosk library with a pre-trained English language model to deliver accurate speech recognition locally within your Mendix environment.

## Features

- 🎯 **Offline Transcription**: No internet connection required
- 🚀 **Fast Processing**: Optimized for performance with lightweight model
- 📱 **Multiple Format Support**: Handles various audio formats (WAV recommended)
- 🔧 **Easy Integration**: Simple Java action for seamless Mendix integration
- 📊 **Detailed Logging**: Comprehensive logging for debugging and monitoring
- 🌐 **English Language**: Pre-configured with English (US) Vosk model

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
- **Required Libraries**: 
  - `vosk-0.3.45.jar` - Vosk speech recognition library
  - `jna-5.17.0.jar` - Java Native Access for native library integration
  - `jackson-annotations-2.19.2.jar` - JSON processing
  - `jackson-core-2.19.2.jar` - JSON processing core
  - `jackson-databind-2.19.2.jar` - JSON data binding
- **Vosk Model**: Pre-trained English language model (`vosk-model-small-en-us-0.15`)

### 3. Usage

#### Basic Implementation

1. **Create a Microflow** in your domain model
2. **Add the TranscribeAudioFile Java Action** to your microflow
3. **Pass a FileDocument** containing your audio file as the parameter
4. **Capture the return value** (String) which contains the transcribed text

#### Example Microflow Steps

```
1. [Start] → 
2. [Retrieve Audio FileDocument] → 
3. [Call TranscribeAudioFile] → 
4. [Process/Store Transcription Result] → 
5. [End]
```

#### Input Requirements

- **Parameter**: FileDocument entity with audio content
- **Supported Formats**: WAV (recommended), MP3, M4A, and other common audio formats
- **Optimal Settings**: 16kHz, 16-bit, mono WAV files for best performance

#### Return Value

- **Type**: String
- **Content**: Transcribed text from the audio file
- **Empty Result**: Returns informative message if no speech is detected

## Technical Details

### Audio Processing

The module automatically:
- Validates audio file formats
- Converts audio to compatible format when necessary
- Processes audio in optimized chunks
- Handles WAV headers appropriately

### Performance

- **Model Size**: ~40MB (small, optimized model)
- **Processing Speed**: Real-time or faster depending on audio length
- **Memory Usage**: Minimal footprint suitable for production environments

### Error Handling

The module provides comprehensive error handling for:
- Invalid or corrupted audio files
- Missing or inaccessible Vosk model
- Insufficient system resources
- Unsupported audio formats

## Requirements

### Mendix Platform
- **Mendix Version**: 10.24.2 or higher
- **Java Version**: Java 11 or higher

### System Requirements
- **RAM**: Minimum 512MB available memory
- **Storage**: 100MB for module and model files
- **Platform**: Windows, Linux, macOS (wherever Mendix runs)

## Deployment

### Local Development
After importing the MPK, the module is ready to use immediately.

### Production Deployment

1. **Include Resources**: Ensure the `resources/vibetranscribe/` directory is included in your deployment
2. **Verify Model**: Confirm the Vosk model files are accessible in the runtime environment
3. **Test Functionality**: Perform transcription tests in your target environment

### Cloud Deployment

For Mendix Cloud deployments:
1. All required JAR files are automatically included
2. Resource files (Vosk model) are packaged with your application
3. No additional configuration needed

## Troubleshooting

### Common Issues

**"Model not found" Error**
- Verify the `resources/vibetranscribe/vosk-model-small-en-us-0.15/` directory exists
- Check deployment package includes resource files
- Review application logs for exact paths being searched

**"No speech detected" Result**
- Ensure audio file contains clear speech
- Verify audio format compatibility (WAV recommended)
- Check audio quality and volume levels

**Performance Issues**
- Use WAV format for optimal processing speed
- Ensure sufficient system memory
- Consider audio file size and length

### Logging

Enable detailed logging by setting the `vibetranscribe` log level to `DEBUG` in your Mendix application settings.

## License

This module includes the following third-party components:
- **Vosk**: Apache License 2.0
- **Jackson**: Apache License 2.0
- **JNA**: LGPL 2.1 / Apache License 2.0

## Support

For questions, issues, or feature requests, please refer to the module documentation or contact the development team.

## Version History

- **1.0.0**: Initial release with Vosk integration and English model
  - Pre-configured with vosk-model-small-en-us-0.15
  - Updated dependencies: Jackson 2.19.2, JNA 5.17.0
  - Optimized for Mendix 10.24.2+ environments