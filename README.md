# Pocket TTS Hindi

<img width="1446" height="622" alt="pocket-tts-logo-v2-transparent" src="https://github.com/user-attachments/assets/637b5ed6-831f-4023-9b4c-741be21ab238" />

A lightweight **local Text-to-Speech (TTS)** implementation based on [Kyutai Pocket TTS](https://github.com/kyutai-labs/pocket-tts), extended with **Hindi language support**.

The goal of this project is to make Pocket TTS capable of generating natural Hindi speech locally on a CPU without requiring a GPU or a cloud TTS API.

> **Hindi support has been tested locally on Windows using CPU inference and int8 quantization.**

---

## ✨ Highlights

- 🖥️ Runs locally on CPU
- 🇮🇳 **Hindi language support**
- 🎙️ Voice cloning using an audio reference
- ⚡ Faster-than-real-time generation on suitable CPUs
- 🧠 Small and lightweight TTS architecture
- 🔊 WAV audio generation
- 💻 CLI support
- 🌐 Local HTTP server support
- 🐍 Python API
- 📦 Hugging Face model/config support
- 🔒 No cloud TTS API required for inference
- 🪟 Tested on Windows
- 🐧 Compatible with CPU-based Linux environments

### Supported languages

The original Pocket TTS languages include:

- English
- French
- German
- Portuguese
- Italian
- Spanish
- Dutch

This fork additionally adds:

- 🇮🇳 **Hindi**

---

# 🇮🇳 Hindi Language Support

This repository adds Hindi support using the community-trained Hindi Pocket TTS model from:

**Saryps Labs — Pocket TTS Hindi**

Hugging Face:

https://huggingface.co/saryps-labs/pocket-tts-hindi

The Hindi model provides:

- Hindi tokenizer
- Hindi model weights
- Hindi-specific Pocket TTS configuration
- CPU-compatible inference
- Voice cloning through an audio reference

The Hindi configuration is included in this repository at:

```text
pocket_tts/config/hindi.yaml
```

The configuration automatically downloads the required Hindi model and tokenizer from Hugging Face.

---

# 🚀 Installation

## Requirements

- Python 3.10–3.14
- PyTorch 2.5+
- CPU
- Git
- Internet connection for the first model download

GPU is **not required**.

---

## Clone the repository

```bash
git clone https://github.com/builderAbhishek/pocket-tts-hindi.git
cd pocket-tts-hindi
```

---

## Create a virtual environment

### Windows

```bash
python -m venv .venv
```

Activate it:

```bash
.venv\Scripts\activate
```

You should see:

```text
(.venv)
```

in your terminal.

---

## Install dependencies

```bash
pip install -e .
```

If additional dependencies are required:

```bash
pip install scipy huggingface_hub
```

---

# 🔊 Generate Speech

## Default Pocket TTS

```bash
pocket-tts generate
```

This generates the default speech output.

---

# 🇮🇳 Generate Hindi Speech

Hindi can be selected directly using:

```bash
pocket-tts generate --language hindi
```

You can provide your own Hindi text:

```bash
pocket-tts generate ^
  --language hindi ^
  --text "नमस्ते, मेरा नाम अभिषेक है। यह हिंदी टेक्स्ट टू स्पीच का परीक्षण है।" ^
  --output-path hindi_output.wav ^
  --device cpu ^
  --quantize
```

### Linux / macOS

```bash
pocket-tts generate \
  --language hindi \
  --text "नमस्ते, मेरा नाम अभिषेक है। यह हिंदी टेक्स्ट टू स्पीच का परीक्षण है।" \
  --output-path hindi_output.wav \
  --device cpu \
  --quantize
```

The generated file will be:

```text
hindi_output.wav
```

---

# 🎙️ Hindi Voice

The Hindi model does not provide a predefined Pocket TTS voice embedding in the same way as the original built-in languages.

Therefore, Hindi uses an audio reference for voice conditioning.

The default Hindi voice uses the **Alba reference audio**:

```text
hf://kyutai/tts-voices/alba-mackenna/casual.wav
```

You can also explicitly provide a voice:

```bash
pocket-tts generate ^
  --language hindi ^
  --voice "hf://kyutai/tts-voices/alba-mackenna/casual.wav" ^
  --text "नमस्ते, मेरा नाम अभिषेक है।" ^
  --output-path hindi_voice.wav ^
  --device cpu ^
  --quantize
```

---

# 🎤 Use Your Own Voice

Pocket TTS can clone a voice from an audio file.

For example:

```bash
pocket-tts generate ^
  --language hindi ^
  --voice "my_voice.wav" ^
  --text "नमस्ते, यह मेरी आवाज़ में हिंदी टेक्स्ट टू स्पीच है।" ^
  --output-path my_hindi_voice.wav ^
  --device cpu ^
  --quantize
```

For better results, use a clean voice recording with:

- One speaker
- Minimal background noise
- Clear speech
- Good microphone quality
- No music
- No heavy echo

> Only clone voices when you have the speaker's explicit and lawful consent.

---

# ⚙️ Hindi Model Configuration

The Hindi configuration is located at:

```text
pocket_tts/
└── config/
    └── hindi.yaml
```

The configuration points to the Hindi model:

```yaml
weights_path: hf://saryps-labs/pocket-tts-hindi/model.safetensors
```

and Hindi tokenizer:

```yaml
tokenizer_path: hf://saryps-labs/pocket-tts-hindi/tokenizer.model
```

This means the model files do not need to be manually committed into the Git repository.

They are downloaded from Hugging Face when Hindi generation is used for the first time.

---

# 🧪 Tested Hindi Generation

Hindi support has been tested locally with:

```bash
pocket-tts generate \
  --language hindi \
  --text "नमस्ते, मेरा नाम अभिषेक है। यह हिंदी टेक्स्ट टू स्पीच का अंतिम परीक्षण है।" \
  --output-path hindi_final_test.wav \
  --device cpu \
  --quantize
```

Example local result:

```text
Generated: 7040 ms of audio in 9188 ms
```

Approximately:

```text
0.77x real-time
```

on the test CPU environment.

This confirms that Hindi speech generation works locally using CPU inference.

---

# 🖥️ Local Server

You can also start the Pocket TTS server:

```bash
pocket-tts serve --language hindi
```

Then open:

```text
http://localhost:8000
```

The server keeps the model loaded in memory, which can make repeated requests faster.

---

# 🧰 CLI Commands

Pocket TTS provides three main commands:

### Generate

```bash
pocket-tts generate
```

Generate speech from text.

### Serve

```bash
pocket-tts serve
```

Start the local HTTP/web interface.

### Export Voice

```bash
pocket-tts export-voice
```

Convert an audio prompt into a reusable voice embedding.

---

# 🐍 Python Usage

Pocket TTS can also be used as a Python library.

Example:

```python
from pocket_tts import TTSModel
import scipy.io.wavfile

tts_model = TTSModel.load_model(language="hindi")

voice_state = tts_model.get_state_for_audio_prompt(
    "my_voice.wav"
)

audio = tts_model.generate_audio(
    voice_state,
    "नमस्ते, यह Python से हिंदी टेक्स्ट टू स्पीच है।"
)

scipy.io.wavfile.write(
    "hindi_output.wav",
    tts_model.sample_rate,
    audio.numpy()
)
```

---

# 🧠 How Hindi Support Works

The Hindi integration adds support at several levels:

```text
Hindi Text
    ↓
Hindi Tokenizer
    ↓
Hindi Pocket TTS Model
    ↓
Audio Conditioning
    ↓
Mimi Decoder
    ↓
WAV Audio
```

The CLI recognizes:

```bash
--language hindi
```

The repository includes:

```text
pocket_tts/config/hindi.yaml
```

and the default language handling has been extended so Hindi can be selected directly from the CLI.

---

# 📁 Project Structure

```text
pocket-tts-hindi/
│
├── pocket_tts/
│   ├── config/
│   │   ├── english.yaml
│   │   ├── french.yaml
│   │   ├── german.yaml
│   │   ├── italian.yaml
│   │   ├── portuguese.yaml
│   │   ├── spanish.yaml
│   │   ├── dutch.yaml
│   │   └── hindi.yaml
│   │
│   ├── models/
│   ├── modules/
│   ├── utils/
│   ├── default_parameters.py
│   └── main.py
│
├── README.md
├── pyproject.toml
└── ...
```

---

# 📦 CPU Quantization

For systems with limited RAM or older CPUs, int8 quantization can reduce memory usage:

```bash
pocket-tts generate \
  --language hindi \
  --text "नमस्ते दुनिया" \
  --device cpu \
  --quantize
```

This is particularly useful for local CPU inference.

> `--quantize` is intended for CPU execution.

---

# 🔐 Hugging Face Authentication

The first Hindi generation downloads the model and tokenizer from Hugging Face.

Without authentication, Hugging Face may display a warning about unauthenticated requests.

The model can still download and work normally.

For higher rate limits, configure a Hugging Face token:

```bash
hf auth login
```

or use the Hugging Face authentication mechanism appropriate for your environment.

---

# 🌐 Original Pocket TTS

This project is based on:

**Kyutai Pocket TTS**

Original repository:

https://github.com/kyutai-labs/pocket-tts

Original project resources:

- [Demo](https://kyutai.org/pocket-tts)
- [GitHub](https://github.com/kyutai-labs/pocket-tts)
- [Hugging Face](https://huggingface.co/kyutai/pocket-tts)
- [Documentation](https://kyutai-labs.github.io/pocket-tts/)
- [Technical Report](https://kyutai.org/blog/2026-01-13-pocket-tts)
- [Paper](https://arxiv.org/abs/2509.06926)

---

# 🤗 Hindi Model

Hindi model used by this project:

**Saryps Labs — Pocket TTS Hindi**

https://huggingface.co/saryps-labs/pocket-tts-hindi

The Hindi model configuration in this repository points to the model hosted by Saryps Labs.

---

# ⚠️ Disclaimer

This repository is an extension/fork of the original Pocket TTS project.

The Hindi model weights are hosted separately by Saryps Labs on Hugging Face.

Please review and follow the applicable licenses for:

- Pocket TTS
- Hindi model weights
- Voice/audio samples
- Any third-party dependencies

Do not use voice cloning for impersonation, fraud, deception, harassment, or any unauthorized purpose.

Always obtain appropriate consent before cloning a person's voice.

---

# 🤝 Contributing

Contributions are welcome.

You can contribute by:

- Adding new language configurations
- Improving Hindi generation quality
- Adding better voice presets
- Improving CPU performance
- Improving documentation
- Adding tests
- Fixing bugs
- Adding examples
- Improving the CLI

Pull requests are welcome.

---

# ⭐ Support the Project

If this project is useful to you:

- ⭐ Star the repository
- 🐛 Report issues
- 💡 Suggest improvements
- 🔧 Submit pull requests
- 📢 Share the project with developers working on local AI and Hindi TTS

---

# 📄 License

Please refer to the original [Kyutai Pocket TTS repository](https://github.com/kyutai-labs/pocket-tts) and the respective model repositories for licensing information.

---

## 👨‍💻 Project

**Pocket TTS Hindi**

GitHub:

https://github.com/builderAbhishek/pocket-tts-hindi

Built as a local Hindi TTS extension of Kyutai Pocket TTS.

**Made for local, CPU-friendly Hindi speech synthesis. 🇮🇳**