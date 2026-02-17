---
name: whisper-skill
description: Process voice messages and audio files through local Whisper model. Use when user sends audio messages, voice notes, or needs speech-to-text transcription. Automatically transcribes Ukrainian, Russian, English and other languages.
---

# Whisper Voice-to-Text Skill

Local speech-to-text processing via OpenAI Whisper (base model). Transcribes voice messages and audio files with ~75% accuracy in real-time.

## How It Works

1. **User sends voice message** → OpenClaw receives audio file
2. **Whisper processes** → Local model transcribes (base = 2GB VRAM, ~2-5 sec per minute of audio)
3. **You get text** → Result sent directly to your chat

## Supported Formats

- MP3, WAV, M4A, OGG, FLAC, WebM
- Any format ffmpeg supports

## Supported Languages

- Ukrainian (uk)
- Russian (ru)  
- English (en)
- Polish (pl)
- And 90+ more languages

## Service Status

### Running Services

**Audio Listener Service** (Background daemon):
```
python C:\Users\Administrator\.openclaw\workspace\audio_listener.py
```

- Monitors: `C:\Users\Administrator\.openclaw\workspace\audio_input/`
- Automatically transcribes new audio files
- Results sent to Telegram
- Processed files moved to: `audio_processed/`

### Manual Transcription

```bash
python whisper_transcribe.py <audio_file> [language]
```

Example:
```bash
python whisper_transcribe.py voice.mp3 uk      # Ukrainian
python whisper_transcribe.py voice.wav en      # English
python whisper_transcribe.py voice.m4a         # Auto-detect
```

## Results Storage

Results stored in: `audio_results.json`

```json
{
  "voice_2026_02_15.mp3": {
    "text": "Привіт, як дела?",
    "timestamp": "2026-02-15T00:15:30.123456",
    "status": "done"
  }
}
```

## Next Steps

1. **Start the listener service:**
   ```bash
   python audio_listener.py
   ```
   
2. **Send voice messages** in Telegram → Auto-transcribed

3. **Check results** in `audio_results.json`

## Troubleshooting

**Service not responding?**
- Check: `python audio_listener.py` is running
- Check: `audio_input/` folder exists
- Check: File permissions

**Poor transcription?**
- Upgrade to `small` model (better accuracy, slower)
- Ensure audio quality is good
- Specify correct language: `python whisper_transcribe.py voice.mp3 uk`

**Model loading slow?**
- First run downloads model (~140MB)
- Subsequent runs use cached model (fast)
