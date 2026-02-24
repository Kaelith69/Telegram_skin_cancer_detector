<div align="center">

<!-- Wiki Home Hero SVG — Healthcare: #0B8F87 · #2563EB · #10B981 -->
<svg xmlns="http://www.w3.org/2000/svg" width="800" height="140" viewBox="0 0 800 140" role="img" aria-label="Wiki Home Banner">
  <defs>
    <linearGradient id="wBg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#062e2c"/>
      <stop offset="100%" stop-color="#0a1f2e"/>
    </linearGradient>
    <linearGradient id="wAcc" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#0B8F87"/>
      <stop offset="100%" stop-color="#10B981"/>
    </linearGradient>
  </defs>
  <rect width="800" height="140" fill="url(#wBg)" rx="10"/>
  <circle cx="730" cy="30" r="55" fill="#0B8F87" opacity="0.08"/>
  <text x="400" y="60"  font-family="Arial, sans-serif" font-size="30" font-weight="700" fill="url(#wAcc)" text-anchor="middle">🩺 Skin Cancer Detector Bot</text>
  <text x="400" y="90"  font-family="Arial, sans-serif" font-size="15" fill="#a8d8d4" text-anchor="middle">Wiki · Documentation Home</text>
  <text x="400" y="112" font-family="Arial, sans-serif" font-size="11" fill="#6ec6f5" text-anchor="middle" font-style="italic">HuggingFace ViT · Google Gemini 2.0 Flash · python-telegram-bot v20</text>
  <rect y="133" width="800" height="5" rx="2" fill="url(#wAcc)" opacity="0.8"/>
</svg>

</div>

---

Welcome to the **Skin Cancer Detector Bot** wiki. This is your one-stop-shop for understanding how the bot works, how to run it, and how to not accidentally commit your API keys.

> **⚕️ Remember:** This is an educational/research tool. Not a replacement for a dermatologist. A dermatologist went to school for 11+ years. The bot was written in Python. Both have their place.

---

## 📚 Wiki Pages

| Page | What You'll Find |
|------|-----------------|
| [Architecture](Architecture.md) | System design, module breakdown, design decisions |
| [Installation](Installation.md) | Step-by-step setup from zero to running bot |
| [Usage](Usage.md) | How to use the bot, example outputs, classifiable conditions |
| [Privacy](Privacy.md) | What happens to your images (spoiler: they disappear) |
| [Troubleshooting](Troubleshooting.md) | Common errors and how to fix them |
| [Roadmap](Roadmap.md) | Planned features and future direction |

---

## ⚡ Quick Start

```bash
git clone https://github.com/Kaelith69/Telegram_skin_cancer_detector.git
cd Telegram_skin_cancer_detector
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # fill in BOT_TOKEN + GEMINI_API_KEY
python bot.py
```

If you see `🤖 Skin Diagnosis Bot is live.` — you're done. Send a photo of a skin lesion to your bot.

---

## 🏗️ What Does This Bot Actually Do?

1. **User sends a photo** of a skin lesion via Telegram.
2. The bot downloads the image to a **temporary file** (not stored anywhere permanent).
3. A **Vision Transformer (ViT)** model from HuggingFace classifies it into one of 7 conditions from the HAM10000 dataset.
4. The predicted label is sent to **Google Gemini 2.0 Flash**, which returns a plain-English explanation, signs, treatment options, and a severity label.
5. The formatted result is sent back to the user via **MarkdownV2** formatted reply.
6. The temp file is **deleted** — unconditionally, in a `finally` block.

Total time: roughly 2 seconds. Total data retained: zero bytes.

---

## 🔧 Tech Stack Summary

| Component | Technology |
|-----------|-----------|
| Bot Framework | python-telegram-bot v20.7 |
| Image Classification | HuggingFace Transformers (ViT-B/16) |
| ML Backend | PyTorch (CPU inference) |
| Medical Insights | Google Gemini 2.0 Flash API |
| Image Processing | Pillow (PIL) |
| Config Management | python-dotenv |
| Deployment | Heroku / Railway (via Procfile) |

---

## ❗ Medical Disclaimer

This bot provides AI-generated predictions for **educational and research purposes only**. It is **not** a medical device. It is **not** a diagnostic tool. It is **not** a substitute for professional medical advice.

If the bot — or any other tool — suggests you might have a serious skin condition, please consult a qualified dermatologist. Seriously. Go.
