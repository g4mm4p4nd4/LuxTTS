# LuxTTS
<p align="center">
  <a href="https://huggingface.co/YatharthS/LuxTTS">
    <img src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Model-FFD21E" alt="Hugging Face Model">
  </a>
  &nbsp;
  <a href="https://huggingface.co/spaces/YatharthS/LuxTTS">
    <img src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Space-blue" alt="Hugging Face Space">
  </a>
  &nbsp;
  <a href="https://colab.research.google.com/drive/1cDaxtbSDLRmu6tRV_781Of_GSjHSo1Cu?usp=sharing">
    <img src="https://img.shields.io/badge/Colab-Notebook-F9AB00?logo=googlecolab&logoColor=white" alt="Colab Notebook">
  </a>
</p>

LuxTTS is an lightweight zipvoice based text-to-speech model designed for high quality voice cloning and realistic generation at speeds exceeding 150x realtime.

https://github.com/user-attachments/assets/a3b57152-8d97-43ce-bd99-26dc9a145c29


### The main features are
- Voice cloning: SOTA voice cloning on par with models 10x larger.
- Clarity: Clear 48khz speech generation unlike most TTS models which are limited to 24khz.
- Speed: Reaches speeds of 150x realtime on a single GPU and faster then realtime on CPU's as well.
- Efficiency: Fits within 1gb vram meaning it can fit in any local gpu.

## Usage
You can try it locally, colab, or spaces.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1cDaxtbSDLRmu6tRV_781Of_GSjHSo1Cu?usp=sharing)
[![Open in Spaces](https://huggingface.co/datasets/huggingface/badges/resolve/main/open-in-hf-spaces-sm.svg)](https://huggingface.co/spaces/YatharthS/LuxTTS)

#### Simple installation:
```
git clone https://github.com/ysharma3501/LuxTTS.git
cd LuxTTS
pip install -r requirements.txt
```

#### Load model:
```python
from zipvoice.luxvoice import LuxTTS
lux_tts = LuxTTS('YatharthS/LuxTTS', device='cuda', threads=2) ## change device to cpu for cpu usage
```

#### Simple inference
```python
import soundfile as sf
from IPython.display import Audio

text = "Hey, what's up? I'm feeling really great if you ask me honestly!"

## change this to your reference file path, can be wav/mp3
prompt_audio = 'audio_file.wav'

## encode audio(takes 10s to init because of librosa first time)
encoded_prompt = lux_tts.encode_prompt(prompt_audio, rms=0.01)

## generate speech
final_wav = lux_tts.generate_speech(text, encoded_prompt, num_steps=num_steps)

## save audio
final_wav = final_wav.numpy().squeeze()
sf.write('output.wav', audio_data, 48000)

## display speech
display(Audio(final_wav, rate=48000))
```

#### Inference with sampling params:
```python
import soundfile as sf
from IPython.display import Audio

text = "Hey, what's up? I'm feeling really great if you ask me honestly!"

## change this to your reference file path, can be wav/mp3
prompt_audio = 'audio_file.wav'

rms = 0.01 ## higher makes it sound louder(0.01 or so recommended)
t_shift = 0.9 ## sampling param, higher can sound better but worse WER
num_steps = 4 ## sampling param, higher sounds better but takes longer(3-4 is best for efficiency)
speed = 1.0 ## sampling param, controls speed of audio(lower=slower)
return_smooth = False ## sampling param, makes it sound smoother possibly but less cleaner
ref_duration = 5 ## Setting it lower can speedup inference, set to 1000 if you find artifacts.

## encode audio(takes 10s to init because of librosa first time)
encoded_prompt = lux_tts.encode_prompt(prompt_audio, duration=ref_duration, rms=rms)

## generate speech
final_wav = lux_tts.generate_speech(text, encoded_prompt, num_steps=num_steps, t_shift=t_shift, speed=speed, return_smooth=return_smooth)

## save audio
final_wav = final_wav.numpy().squeeze()
sf.write('output.wav', audio_data, 48000)

## display speech
display(Audio(final_wav, rate=48000))
```
## Tips
- Please use at minimum a 3 second audio file for voice cloning.
- You can use return_smooth = True if you hear metallic sounds.
- Lower t_shift for less possible pronunciation errors but worse quality and vice versa.

  
## Info

Q: How is this different from ZipVoice?

A: LuxTTS uses the same architecture but distilled to 4 steps with an improved sampling technique. It also uses a custom 48khz vocoder instead of the default 24khz version.

Q: Can it be even faster?

A: Yes, currently it uses float32. Float16 should be significantly faster(almost 2x).

## Roadmap

- [x] Release model and code
- [x] Huggingface spaces demo
- [ ] Release mps support
- [ ] Release code for float16 inference

## Acknowledgments

- [ZipVoice](https://github.com/k2-fsa/ZipVoice) for their excellent code and model.
- [Vocos](https://github.com/gemelo-ai/vocos.git) for their great vocoder.
  
## Overview & Purpose

LuxTTS is an open‑source text‑to‑speech system based on ZipVoice, designed to deliver high‑quality voice cloning and natural speech synthesis at incredibly fast speeds. It serves as a lightweight solution for generating lifelike audio for applications ranging from personal assistants and audiobooks to content creation and accessibility tools. By offering 48 kHz audio output and optimized sampling techniques, LuxTTS aims to make advanced voice synthesis accessible to developers and researchers alike.

## Features & Tech Stack

Key features include:

- **High‑fidelity voice cloning**: Clones voices with clarity comparable to much larger models by using a distilled 4‑step architecture and a custom 48 kHz vocoder.
- **Real‑time performance**: Delivers generation speeds up to 150× realtime on a single GPU and performs competitively on CPUs, enabling responsive applications.
- **Lightweight & efficient**: Fits within ≈1 GB VRAM, allowing deployment on consumer GPUs and local machines.
- **Configurable inference**: Provides parameters (rms, t_shift, num_steps, speed, return_smooth, ref_duration) to balance quality and performance for different use cases.
- **Extensible**: Built using modular Python code so it can be integrated into larger voice AI pipelines or wrapped as an API.

Tech stack:

| Technology | Role |
| --- | --- |
| Python | Primary language for model code and scripts |
| PyTorch & Torchaudio | Deep learning framework and audio processing |
| NumPy | Numerical operations during processing |
| IPython & Jupyter | Interactive demos and notebook execution |

## Installation & Usage

Follow these steps to get started locally:

```bash
git clone https://github.com/g4mm4p4nd4/LuxTTS.git
cd LuxTTS
pip install -r requirements.txt
```

You can then load the model and generate speech in Python:

```python
from zipvoice.luxvoice import LuxTTS

# instantiate the model (use 'cpu' for CPU inference)
lux_tts = LuxTTS('Yatharths/LuxTTS', device='cuda', threads=2)

# encode a reference audio prompt (3 sec WAV/MP3)
prompt_path = 'audio_file.wav'
encoded_prompt = lux_tts.encode_prompt(prompt_path, rms=0.01)

# generate speech from text and the encoded prompt
text = "Hello, this is a LuxTTS demo."
generated = lux_tts.generate_speech(text, encoded_prompt, num_steps=4)

# save the result
generated.save('output.wav')
```

## Business & Entrepreneurial Value

The LuxTTS project opens several monetization opportunities:

- **Licensing & subscriptions**: Offer the voice‑cloning technology as a paid API or SaaS to developers building voice assistants, audiobooks, customer‑service bots or content generation tools.
- **Custom integrations & upsells**: Embed LuxTTS into existing platforms (podcast hosting, language learning apps, media production suites) and sell premium voices, priority compute tiers or enterprise support.
- **Efficiency & scalability**: Its lightweight footprint and rapid inference enable cost‑effective scaling and the ability to serve many users without large infrastructure, lowering operating expenses.
- **Partnerships**: Collaborate with hardware manufacturers or edge‑AI vendors to bundle LuxTTS for offline voice synthesis, creating OEM licensing revenue.

## Consumer Value

End users gain clear benefits from LuxTTS‑powered applications:

- **Natural & personalized audio**: Delivers high‑quality, 48 kHz speech that sounds realistic and can be tailored to individual voices or styles.
- **Fast results**: Users receive synthesized speech quickly, improving experience in interactive interfaces like chatbots, reading assistants and accessibility tools.
- **Accessible & privacy‑friendly**: Runs on consumer hardware, allowing private local inference without sending sensitive voice data to external servers.
- **Customization & control**: Adjustable parameters give power users the ability to balance audio quality, speed and resource usage according to their needs.

## Final Notes

The model and code are licensed under the Apache-2.0 license. See LICENSE for details.

Stars/Likes would be appreciated, thank you.

Email: yatharthsharma350@gmail.com
