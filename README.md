<div align="center">

<!-- SVG Hero Banner -->
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 900 220" width="900" height="220" role="img" aria-label="Skin Cancer Detector Bot Banner">
  <defs>
    <linearGradient id="bg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%"   stop-color="#0f2027"/>
      <stop offset="50%"  stop-color="#203a43"/>
      <stop offset="100%" stop-color="#2c5364"/>
    </linearGradient>
    <linearGradient id="accent" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%"   stop-color="#00c9ff"/>
      <stop offset="100%" stop-color="#92fe9d"/>
    </linearGradient>
    <filter id="glow">
      <feGaussianBlur stdDeviation="3" result="blur"/>
      <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
    </filter>
  </defs>
  <!-- Background -->
  <rect width="900" height="220" fill="url(#bg)" rx="16"/>
  <!-- Decorative circles -->
  <circle cx="820" cy="40"  r="60" fill="#00c9ff" opacity="0.07"/>
  <circle cx="80"  cy="180" r="80" fill="#92fe9d" opacity="0.06"/>
  <!-- DNA helix dots (decorative) -->
  <g opacity="0.25" fill="#00c9ff">
    <circle cx="760" cy="80"  r="4"/><circle cx="780" cy="100" r="4"/>
    <circle cx="800" cy="120" r="4"/><circle cx="780" cy="140" r="4"/>
    <circle cx="760" cy="160" r="4"/>
  </g>
  <g opacity="0.25" fill="#92fe9d">
    <circle cx="800" cy="80"  r="4"/><circle cx="780" cy="100" r="4"/>
    <circle cx="760" cy="120" r="4"/><circle cx="780" cy="140" r="4"/>
    <circle cx="800" cy="160" r="4"/>
  </g>
  <!-- Microscope icon (simplified) -->
  <g transform="translate(60,55)" filter="url(#glow)">
    <rect x="14" y="0"  width="8" height="30" rx="4" fill="#00c9ff"/>
    <rect x="8"  y="28" width="20" height="6" rx="3" fill="#00c9ff"/>
    <ellipse cx="18" cy="60" rx="14" ry="5" fill="none" stroke="#00c9ff" stroke-width="2"/>
    <line x1="4" y1="65" x2="32" y2="65" stroke="#00c9ff" stroke-width="2"/>
    <circle cx="18" cy="14" r="8" fill="none" stroke="#92fe9d" stroke-width="2.5"/>
  </g>
  <!-- Title -->
  <text x="155" y="100" font-family="'Segoe UI', Arial, sans-serif" font-size="38"
        font-weight="700" fill="url(#accent)" filter="url(#glow)">
    Skin Cancer Detector
  </text>
  <!-- Subtitle -->
  <text x="157" y="135" font-family="'Segoe UI', Arial, sans-serif" font-size="18"
        fill="#c9d6df" letter-spacing="1">
    Telegram Bot · AI-Powered Dermatology Assistant
  </text>
  <!-- Tag line -->
  <text x="157" y="168" font-family="'Segoe UI', Arial, sans-serif" font-size="13"
        fill="#6ec6f5" font-style="italic">
    HuggingFace · Gemini · python-telegram-bot v20
  </text>
  <!-- Bottom accent bar -->
  <rect y="208" width="900" height="6" rx="3" fill="url(#accent)" opacity="0.8"/>
</svg>

<br/>

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![python-telegram-bot](https://img.shields.io/badge/python--telegram--bot-20.7-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://python-telegram-bot.org/)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)](https://huggingface.co/)
[![Gemini](https://img.shields.io/badge/Google-Gemini%202.0-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://aistudio.google.com/)
[![License](https://img.shields.io/badge/License-Educational%20Use-green?style=for-the-badge)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.1.0-blueviolet?style=for-the-badge)](CHANGELOG)

> **⚠️ Medical Disclaimer:** This bot is a research/educational tool. It is **not** a substitute for professional medical advice, diagnosis, or treatment. Always consult a qualified dermatologist.

</div>

---

## 📖 Table of Contents

- [✨ Features](#-features)
- [🏗️ Architecture](#️-architecture)
- [📁 Project Structure](#-project-structure)
- [⚙️ Setup & Installation](#️-setup--installation)
- [🚀 Usage](#-usage)
- [🧬 The Model](#-the-model)
- [🔧 Configuration](#-configuration)
- [🤝 Contributing](#-contributing)
- [⚠️ Disclaimer](#️-disclaimer)
- [�� License](#-license)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔬 **Image Classification** | Classifies skin lesion photos into 9 diagnostic categories using a fine-tuned ViT model |
| 📊 **Confidence Score** | Reports the softmax probability of the top prediction so you know how sure the AI is |
| 🧠 **AI Medical Insights** | Queries the Gemini 2.0 Flash API for a plain-English explanation, signs, treatment, and next steps |
| ⚠️ **Safety Label** | Classifies the condition as Cancerous / Pre-cancerous / Non-cancerous at a glance |
| 🔒 **Ephemeral Processing** | Downloaded images are stored in a secure OS temp directory and deleted immediately after analysis |
| 🛡️ **Graceful Error Handling** | Network timeouts, API failures, and model errors all fall back to user-friendly messages |

---

## 🏗️ Architecture

```
User (Telegram)
      │  sends photo
      ▼
┌─────────────────────────────────────┐
│        python-telegram-bot v20       │
│  ApplicationBuilder → run_polling() │
│  MessageHandler(filters.PHOTO)      │
└────────────────┬────────────────────┘
                 │ classify_image()
                 ▼
┌─────────────────────────────────────┐
│        HuggingFace Transformers      │
│  AutoImageProcessor (ViT)           │
│  AutoModelForImageClassification    │
│  → softmax → label + confidence     │
└────────────────┬────────────────────┘
                 │ label
                 ▼
┌─────────────────────────────────────┐
│       Google Gemini 2.0 Flash API    │
│  fetch_condition_info(label)        │
│  → safety_label + explanation       │
└────────────────┬────────────────────┘
                 │ format_prediction()
                 ▼
         Reply to user (MarkdownV2)
```

**Key Design Decisions**

- `run_polling()` is the synchronous PTB v20 entry-point — no manual asyncio event loop needed.
- Temporary images are created with `tempfile.mkstemp()` to prevent filename collisions under concurrent requests and to guarantee cleanup in a `finally` block.
- Environment variables are validated at startup so misconfigured deployments fail fast with a clear message.
- The Gemini HTTP call carries a 15-second timeout to prevent the bot from stalling indefinitely.

---

## 📁 Project Structure

```
Telegram_skin_cancer_detector/
├── bot.py              # Main bot logic (classification + Gemini integration)
├── requirements.txt    # Python dependencies
├── Procfile            # Heroku/Railway worker declaration
├── .env.example        # Template for required environment variables
├── .gitignore          # VCS ignore rules (excludes .env, __pycache__, etc.)
└── README.md           # This file
```

---

## ⚙️ Setup & Installation

### Prerequisites

- Python **3.10+**
- A [Telegram bot token](https://t.me/BotFather)
- A [Google Gemini API key](https://aistudio.google.com/app/apikey)

### Step 1 — Clone the repository

```bash
git clone https://github.com/Kaelith69/Telegram_skin_cancer_detector.git
cd Telegram_skin_cancer_detector
```

### Step 2 — Create a virtual environment (recommended)

```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
```

### Step 3 — Install dependencies

```bash
pip install -r requirements.txt
```

### Step 4 — Configure environment variables

```bash
cp .env.example .env
```

Open `.env` and fill in your credentials:

```dotenv
BOT_TOKEN=123456789:ABCdefGHIjklMNOpqrSTUvwxYZ
GEMINI_API_KEY=AIzaSy...
```

> **Never commit your `.env` file.** It is already listed in `.gitignore`.

### Step 5 — Run the bot

```bash
python bot.py
```

You should see:

```
🤖 Skin Diagnosis Bot is live.
```

---

## 🚀 Usage

<div align="center">

<!-- 💡 Great spot for a GIF — drop a short screen-recording of the bot in action here! -->
<!-- Example: ![Bot Demo](https://your-cdn.com/demo.gif) -->

</div>

1. Open Telegram and search for your bot by the username you set with BotFather.
2. Send **any photo** of a skin lesion directly in the chat.
3. The bot will reply with a structured diagnosis:

```
🧾 Diagnosis Result
• Condition:   Melanocytic nevi
• Confidence:  94.73%
• Type:        ✅ Harmless - Non-Cancerous

Medical Insight:
Melanocytic nevi, commonly known as moles, are benign ...
Signs: Small, round, evenly pigmented spots ...
Treatment: Usually no treatment needed ...
What to do: Monitor for ABCDE changes and consult a dermatologist annually.
```

4. If you send a non-photo message, the bot will politely ask you to send an image.

### Classifiable Conditions

The underlying ViT model (`Anwarkh1/Skin_Cancer-Image_Classification`) recognises the following [HAM10000](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DBW86T) categories:

| Abbreviation | Full Name |
|---|---|
| akiec | Actinic Keratoses / Intraepithelial Carcinoma |
| bcc | Basal Cell Carcinoma |
| bkl | Benign Keratosis-like Lesions |
| df | Dermatofibroma |
| mel | Melanoma |
| nv | Melanocytic Nevi |
| vasc | Vascular Lesions |

---

## 🧬 The Model

| Property | Value |
|---|---|
| **Hub ID** | `Anwarkh1/Skin_Cancer-Image_Classification` |
| **Architecture** | Vision Transformer (ViT) |
| **Framework** | HuggingFace Transformers + PyTorch |
| **Input** | RGB image, resized & normalised by `AutoImageProcessor` |
| **Output** | Probability distribution over condition classes |
| **Inference** | CPU-only, wrapped in `torch.no_grad()` for efficiency |

---

## 🔧 Configuration

| Variable | Required | Description |
|---|---|---|
| `BOT_TOKEN` | ✅ | Telegram bot token from [@BotFather](https://t.me/BotFather) |
| `GEMINI_API_KEY` | ✅ | Google Gemini API key from [AI Studio](https://aistudio.google.com/app/apikey) |

Both variables are **validated at startup**. The bot exits immediately with a clear error message if either is missing.

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m 'feat: add my feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

Please open an issue first to discuss significant changes.

---

## ⚠️ Disclaimer

**This bot is not a medical device and does not provide medical advice.**

- Results are AI predictions and may be incorrect.
- Do **not** use this tool as a substitute for professional dermatological examination.
- If the bot flags a potentially serious condition, please consult a qualified doctor promptly.
- The authors accept no liability for decisions made based on the bot's output.

---

## 📄 License

This project is released for **educational and research purposes only**.

---

<div align="center">

*Built with 🤍 and way too much caffeine.*

> **Dad Joke of the Day 🥁**
> Why did the Python developer break up with the JavaScript developer?
> Because they had too many *unresolved promises*. 🥁��

</div>
