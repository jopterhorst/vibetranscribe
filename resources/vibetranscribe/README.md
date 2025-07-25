# VibeTranscribe Resources

This directory contains the Vosk speech recognition model and related resources for the VibeTranscribe module.

## Contents

- `vosk-model-small-en-us-0.15/` - Lightweight English Vosk model (40MB)
  - Optimized for desktop and mobile applications
  - Supports 16kHz mono WAV audio files
  - Provides accurate English speech recognition

## Model Information

- **Version**: 0.15
- **Language**: English (US)
- **Size**: ~40MB
- **Type**: Small model (optimized for resource-constrained environments)
- **Accuracy**: Good balance between size and recognition quality

## Usage

This model is automatically loaded by the `TranscribeAudioFile` Java action. The action will search for the model in this location first, then fall back to other standard locations if needed.

## Deployment

When deploying your Mendix application:
1. Ensure this entire `resources/vibetranscribe/` directory is included in your deployment package
2. The model files must maintain their directory structure
3. For cloud deployments, verify that resource files are properly included

## Model Updates

To update the Vosk model:
1. Download a new model from https://alphacephei.com/vosk/models
2. Extract it to replace the current `vosk-model-small-en-us-0.15/` directory
3. Update the path in `TranscribeAudioFile.java` if the model name changes

## Troubleshooting

If you encounter "model not found" errors:
1. Verify this directory exists in your deployed application
2. Check the logs for the exact paths being searched
3. Ensure file permissions allow read access to the model files
