# Speech Processing Toolkit

A collection of Python scripts for speech processing tasks including transcription, diarization, voice assistance, and speech-to-speech translation.

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![Hugging Face](https://img.shields.io/badge/🤗%20Hugging%20Face-Models-yellow.svg)](https://huggingface.co/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
  - [Meeting Transcription](#meeting-transcription)
  - [Voice Assistant](#voice-assistant)
  - [Speech-to-Speech Translation](#speech-to-speech-translation)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Contributing](#contributing)
- [License](#license)

## Overview

This repository contains a collection of speech processing tools built using Hugging Face Transformers and PyAnnote libraries. The toolkit enables various audio processing capabilities such as speaker diarization, speech recognition, voice assistants, and speech-to-speech translation.

## Features

- **Meeting Transcription with Speaker Diarization**: Transcribe multi-speaker audio and identify who said what.
- **Voice Assistant (Marvin)**: A wake-word activated voice assistant that can respond to queries.
- **Speech-to-Speech Translation**: Translate spoken audio from one language to another, preserving the speech format.

## Installation

```bash
# Clone the repository
git clone https://github.com/AhemdMahmoud/TriVox.git
cd TriVox

# Set up a virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install requirements
pip install -r requirements.txt
```

### Required libraries:

Create a `requirements.txt` file with:

```
transformers>=4.30.0
datasets>=2.10.0
pyannote.audio>=2.1.0
torch>=1.13.0
soundfile>=0.12.1
huggingface_hub>=0.14.0
gradio>=3.20.0
numpy>=1.22.0
requests>=2.28.0
```

## Usage

### Meeting Transcription

The `transcribe_a_meeting.py` script processes audio files to identify speakers and transcribe their speech.

```python
# Example usage
from speechbox import ASRDiarizationPipeline
from transformers import pipeline
from pyannote.audio import Pipeline

# Load pipelines
diarization_pipeline = Pipeline.from_pretrained("pyannote/speaker-diarization@2.1")
asr_pipeline = pipeline("automatic-speech-recognition", model="openai/whisper-base")
combined_pipeline = ASRDiarizationPipeline(asr_pipeline=asr_pipeline, diarization_pipeline=diarization_pipeline)

# Process an audio file
results = combined_pipeline("path/to/your/audio.wav")

# Format output
for chunk in results:
    print(f"{chunk['speaker']} ({chunk['timestamp'][0]:.1f}-{chunk['timestamp'][1]:.1f}): {chunk['text']}")
```

### Voice Assistant

The `creating_a_voice_assistant.py` script implements a wake-word activated voice assistant named Marvin.

```python
# Basic workflow:
# 1. Listen for wake word "marvin"
launch_fn()  # Waits for wake word "marvin"

# 2. Transcribe speech
transcription = transcribe()  # Records and transcribes user's speech

# 3. Generate response using language model
response = query(transcription)  # Generates response text

# 4. Convert response to speech
audio = synthesise(response)  # Converts text response to speech

# Play the response
# In a script, you would use a library like sounddevice to play the audio
```

### Speech-to-Speech Translation

The `speech_to_speech.py` script enables translation of speech from one language to another.

```python
# Example usage
import soundfile as sf

# Load audio file
audio, sample_rate = sf.read("input_audio.wav")

# Translate speech
output_sample_rate, translated_speech = speech_to_speech_translation({"array": audio, "sampling_rate": sample_rate})

# Save output
sf.write("translated_speech.wav", translated_speech, output_sample_rate)
```

## Project Structure

```
.
├── README.md
├── requirements.txt
├── transcribe_a_meeting.py      # Speaker diarization and transcription
├── creating_a_voice_assistant.py # Wake word detection and voice assistant
└── speech_to_speech.py          # Speech-to-speech translation
```

## Requirements

- Python 3.7+
- Hugging Face account for API access
- GPU recommended for faster processing (CUDA compatible)
- FFmpeg for microphone access in voice assistant features

## Authentication

For accessing certain Hugging Face models, you'll need to authenticate:

```python
from huggingface_hub import notebook_login
notebook_login()
# Or use HF_TOKEN environment variable in production
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
