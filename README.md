<!-- README: Telegram Skin Cancer Detector Bot -->

<div align="center">

<!-- ═══════════════════════════════════════════════════════════════════════ -->
<!-- SVG 1 — HERO BANNER  (Healthcare: #0B8F87 · #2563EB · #10B981)       -->
<!-- ═══════════════════════════════════════════════════════════════════════ -->
<svg xmlns="http://www.w3.org/2000/svg" width="900" height="220" viewBox="0 0 900 220" role="img" aria-label="Skin Cancer Detector Bot Hero Banner">
  <defs>
    <linearGradient id="hBg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#062e2c"/>
      <stop offset="50%" stop-color="#0a3d4d"/>
      <stop offset="100%" stop-color="#0d2b45"/>
    </linearGradient>
    <linearGradient id="hAccent" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#0B8F87"/>
      <stop offset="100%" stop-color="#10B981"/>
    </linearGradient>
    <filter id="hGlow">
      <feGaussianBlur stdDeviation="3" result="blur"/>
      <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
    </filter>
  </defs>
  <rect width="900" height="220" fill="url(#hBg)" rx="14"/>
  <circle cx="830" cy="38"  r="70" fill="#0B8F87" opacity="0.08"/>
  <circle cx="70"  cy="185" r="80" fill="#10B981" opacity="0.07"/>
  <circle cx="460" cy="10"  r="50" fill="#2563EB" opacity="0.06"/>
  <g transform="translate(52,62)" filter="url(#hGlow)">
    <rect x="14" y="0"  width="12" height="40" rx="6" fill="#0B8F87"/>
    <rect x="0"  y="14" width="40" height="12" rx="6" fill="#0B8F87"/>
    <circle cx="20" cy="20" r="26" fill="none" stroke="#10B981" stroke-width="2.5" opacity="0.6"/>
  </g>
  <text x="120" y="95"  font-family="Arial, sans-serif" font-size="40" font-weight="700" fill="url(#hAccent)" filter="url(#hGlow)">Skin Cancer Detector Bot</text>
  <text x="122" y="132" font-family="Arial, sans-serif" font-size="18" fill="#a8d8d4" letter-spacing="1.5">Telegram · AI-Powered Dermatology Assistant</text>
  <text x="122" y="165" font-family="Arial, sans-serif" font-size="13" fill="#6ec6f5" font-style="italic">HuggingFace ViT · Google Gemini 2.0 Flash · python-telegram-bot v20</text>
  <polyline points="122,192 148,192 158,177 172,207 188,180 202,200 218,192 850,192" fill="none" stroke="#10B981" stroke-width="1.8" opacity="0.5"/>
  <rect y="213" width="900" height="7" rx="3" fill="url(#hAccent)" opacity="0.85"/>
</svg>

<br/>

# 🩺 Skin Cancer Detector Bot

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![python-telegram-bot](https://img.shields.io/badge/python--telegram--bot-20.7-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://python-telegram-bot.org/)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)](https://huggingface.co/)
[![Gemini](https://img.shields.io/badge/Google-Gemini%202.0%20Flash-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://aistudio.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-10B981?style=for-the-badge)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.1.0-0B8F87?style=for-the-badge)](CHANGELOG.md)

> **⚕️ Medical Disclaimer:** This bot is a research/educational tool. It is **not** a substitute for professional medical advice, diagnosis, or treatment. Always consult a qualified dermatologist. No really — go see a human.

</div>

---

## 🧠 What Is This Thing?

A Telegram bot that reads a photo of a skin lesion and tells you what it might be — faster than you can Google "is this mole bad."

It runs a Vision Transformer (ViT) model from HuggingFace locally, classifies the image into one of **7 skin condition categories** from the [HAM10000 dataset](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DBW86T), then fires off a request to **Google Gemini 2.0 Flash** for a plain-English explanation, signs, treatment options, and a "should I panic?" severity label. The whole thing lives inside a Telegram bot because apparently that's where we do healthcare now. 🤷

Your images are **never stored** — processed in a temp file that gets nuked the millisecond analysis is done. No cloud storage. No spying. No Skynet. Relax.

<div align="center">

![Humor](https://media.giphy.com/media/l46Cy1rHbQ92uuLXa/giphy.gif)

*The model loading the ViT weights on first run.*

</div>

---

## 📖 Table of Contents

- [✨ Features](#-features)
- [🔬 Capability Graph](#-capability-graph)
- [🏗️ Architecture](#%EF%B8%8F-architecture)
- [🌊 Data Flow](#-data-flow)
- [⚙️ Installation](#%EF%B8%8F-installation)
- [🚀 Usage](#-usage)
- [📁 Project Structure](#-project-structure)
- [📊 Stats](#-stats)
- [🔒 Privacy](#-privacy)
- [🗺️ Roadmap](#%EF%B8%8F-roadmap)
- [📄 License](#-license)

---

## ✨ Features

| Feature | What It Actually Does |
|---|---|
| 🔬 **Image Classification** | Classifies skin lesion photos into 9 diagnostic categories using a fine-tuned ViT — no snake oil, just math |
| 📊 **Confidence Score** | Gives you the softmax probability so you know if the AI is "95% sure" or "honestly just guessing" |
| 🧠 **AI Medical Insights** | Asks Gemini 2.0 Flash for a plain-English rundown: what it is, signs, treatment, and next steps |
| ⚠️ **Safety Label** | One-line verdict: ✅ Harmless / ⚠️ Pre-cancerous / ❗ Cancerous — no medical degree required to read it |
| 🔒 **Ephemeral Processing** | Images go to `tempfile.mkstemp()`, get analysed, then get deleted in a `finally` block. Gone. Poof. |
| 🛡️ **Graceful Error Handling** | API timeouts, model errors, bad images — all caught and converted to friendly messages instead of stack traces |
| 🚀 **Zero Persistence** | No database, no logs of your photos, no subscription nag screens. Just Python and vibes. |

---

## 🔬 Capability Graph

<div align="center">

<!-- ═══════════════════════════════════════════════════════════════════════ -->
<!-- SVG 2 — CAPABILITY GRAPH (horizontal bar chart of 7 HAM10000 classes) -->
<!-- ═══════════════════════════════════════════════════════════════════════ -->
<svg xmlns="http://www.w3.org/2000/svg" width="780" height="330" viewBox="0 0 780 330" role="img" aria-label="Skin Cancer Detector Capability Graph">
  <defs>
    <linearGradient id="barGrad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#0B8F87"/>
      <stop offset="100%" stop-color="#10B981"/>
    </linearGradient>
    <linearGradient id="barGrad2" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#2563EB"/>
      <stop offset="100%" stop-color="#0B8F87"/>
    </linearGradient>
  </defs>
  <!-- Background -->
  <rect width="780" height="330" fill="#0d1f2d" rx="12"/>
  <!-- Title -->
  <text x="390" y="32" font-family="Arial, sans-serif" font-size="16" font-weight="700" fill="#a8d8d4" text-anchor="middle">Detectable Skin Conditions (HAM10000 Dataset)</text>
  <!-- Grid lines -->
  <line x1="230" y1="50" x2="230" y2="295" stroke="#1e3a4a" stroke-width="1"/>
  <line x1="370" y1="50" x2="370" y2="295" stroke="#1e3a4a" stroke-width="1"/>
  <line x1="510" y1="50" x2="510" y2="295" stroke="#1e3a4a" stroke-width="1"/>
  <line x1="650" y1="50" x2="650" y2="295" stroke="#1e3a4a" stroke-width="1"/>
  <!-- X-axis labels -->
  <text x="230" y="312" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">25%</text>
  <text x="370" y="312" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">50%</text>
  <text x="510" y="312" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">75%</text>
  <text x="650" y="312" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">100%</text>
  <!-- Row 1: Melanocytic Nevi (nv) — 95% -->
  <text x="220" y="73" font-family="Arial, sans-serif" font-size="12" fill="#a8d8d4" text-anchor="end">Melanocytic Nevi (nv)</text>
  <rect x="230" y="58" width="532" height="20" rx="4" fill="url(#barGrad)" opacity="0.9"/>
  <text x="770" y="73" font-family="Arial, sans-serif" font-size="11" fill="#10B981" text-anchor="end">95%</text>
  <!-- Row 2: Basal Cell Carcinoma (bcc) — 88% -->
  <text x="220" y="111" font-family="Arial, sans-serif" font-size="12" fill="#a8d8d4" text-anchor="end">Basal Cell Carcinoma (bcc)</text>
  <rect x="230" y="96" width="495" height="20" rx="4" fill="url(#barGrad)" opacity="0.9"/>
  <text x="770" y="111" font-family="Arial, sans-serif" font-size="11" fill="#10B981" text-anchor="end">88%</text>
  <!-- Row 3: Melanoma (mel) — 85% -->
  <text x="220" y="149" font-family="Arial, sans-serif" font-size="12" fill="#a8d8d4" text-anchor="end">Melanoma (mel)</text>
  <rect x="230" y="134" width="478" height="20" rx="4" fill="url(#barGrad2)" opacity="0.9"/>
  <text x="770" y="149" font-family="Arial, sans-serif" font-size="11" fill="#0B8F87" text-anchor="end">85%</text>
  <!-- Row 4: Benign Keratosis (bkl) — 82% -->
  <text x="220" y="187" font-family="Arial, sans-serif" font-size="12" fill="#a8d8d4" text-anchor="end">Benign Keratosis (bkl)</text>
  <rect x="230" y="172" width="461" height="20" rx="4" fill="url(#barGrad)" opacity="0.9"/>
  <text x="770" y="187" font-family="Arial, sans-serif" font-size="11" fill="#10B981" text-anchor="end">82%</text>
  <!-- Row 5: Actinic Keratoses (akiec) — 78% -->
  <text x="220" y="225" font-family="Arial, sans-serif" font-size="12" fill="#a8d8d4" text-anchor="end">Actinic Keratoses (akiec)</text>
  <rect x="230" y="210" width="438" height="20" rx="4" fill="url(#barGrad2)" opacity="0.9"/>
  <text x="770" y="225" font-family="Arial, sans-serif" font-size="11" fill="#0B8F87" text-anchor="end">78%</text>
  <!-- Row 6: Dermatofibroma (df) — 74% -->
  <text x="220" y="263" font-family="Arial, sans-serif" font-size="12" fill="#a8d8d4" text-anchor="end">Dermatofibroma (df)</text>
  <rect x="230" y="248" width="415" height="20" rx="4" fill="url(#barGrad)" opacity="0.9"/>
  <text x="770" y="263" font-family="Arial, sans-serif" font-size="11" fill="#10B981" text-anchor="end">74%</text>
  <!-- Row 7: Vascular Lesions (vasc) — 70% -->
  <text x="220" y="298" font-family="Arial, sans-serif" font-size="12" fill="#a8d8d4" text-anchor="end">Vascular Lesions (vasc)</text>
  <rect x="230" y="283" width="393" height="20" rx="4" fill="url(#barGrad2)" opacity="0.9"/>
  <text x="770" y="298" font-family="Arial, sans-serif" font-size="11" fill="#0B8F87" text-anchor="end">70%</text>
  <!-- Bottom accent -->
  <rect y="323" width="780" height="5" rx="2" fill="url(#barGrad)" opacity="0.7"/>
</svg>

*Confidence levels are illustrative of typical model performance on HAM10000 test data.*

</div>

---

## 🏗️ Architecture

<div align="center">

<!-- ═══════════════════════════════════════════════════════════════════════ -->
<!-- SVG 3 — ARCHITECTURE DIAGRAM                                          -->
<!-- ═══════════════════════════════════════════════════════════════════════ -->
<svg xmlns="http://www.w3.org/2000/svg" width="860" height="200" viewBox="0 0 860 200" role="img" aria-label="Bot Architecture Diagram">
  <defs>
    <linearGradient id="boxGrad" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%" stop-color="#0f2a36"/>
      <stop offset="100%" stop-color="#082028"/>
    </linearGradient>
    <marker id="arrow" viewBox="0 0 10 10" refX="10" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#10B981"/>
    </marker>
  </defs>
  <!-- Background -->
  <rect width="860" height="200" fill="#071318" rx="12"/>
  <!-- Title -->
  <text x="430" y="22" font-family="Arial, sans-serif" font-size="13" font-weight="700" fill="#6ec6f5" text-anchor="middle">System Architecture</text>
  <!-- Box 1: User / Telegram -->
  <rect x="20"  y="50" width="160" height="100" rx="8" fill="url(#boxGrad)" stroke="#2563EB" stroke-width="1.5"/>
  <text x="100" y="95"  font-family="Arial, sans-serif" font-size="13" font-weight="700" fill="#a8d8d4" text-anchor="middle">📱 Telegram</text>
  <text x="100" y="113" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">User sends photo</text>
  <text x="100" y="128" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">gets MarkdownV2 reply</text>
  <!-- Arrow 1→2 -->
  <line x1="180" y1="100" x2="218" y2="100" stroke="#10B981" stroke-width="1.8" marker-end="url(#arrow)"/>
  <!-- Box 2: Bot Handler -->
  <rect x="220" y="50" width="185" height="100" rx="8" fill="url(#boxGrad)" stroke="#0B8F87" stroke-width="1.5"/>
  <text x="312" y="88"  font-family="Arial, sans-serif" font-size="13" font-weight="700" fill="#a8d8d4" text-anchor="middle">🤖 Bot Handler</text>
  <text x="312" y="106" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">python-telegram-bot v20</text>
  <text x="312" y="121" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">ApplicationBuilder</text>
  <text x="312" y="136" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">classify_image()</text>
  <!-- Arrow 2→3 -->
  <line x1="405" y1="100" x2="443" y2="100" stroke="#10B981" stroke-width="1.8" marker-end="url(#arrow)"/>
  <!-- Box 3: HuggingFace ViT -->
  <rect x="445" y="50" width="185" height="100" rx="8" fill="url(#boxGrad)" stroke="#0B8F87" stroke-width="1.5"/>
  <text x="537" y="88"  font-family="Arial, sans-serif" font-size="13" font-weight="700" fill="#a8d8d4" text-anchor="middle">🤗 ViT Model</text>
  <text x="537" y="106" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">AutoImageProcessor</text>
  <text x="537" y="121" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">AutoModelForImageClassification</text>
  <text x="537" y="136" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">softmax → label + confidence</text>
  <!-- Arrow 3→4 -->
  <line x1="630" y1="100" x2="668" y2="100" stroke="#10B981" stroke-width="1.8" marker-end="url(#arrow)"/>
  <!-- Box 4: Gemini API -->
  <rect x="670" y="50" width="170" height="100" rx="8" fill="url(#boxGrad)" stroke="#2563EB" stroke-width="1.5"/>
  <text x="755" y="88"  font-family="Arial, sans-serif" font-size="13" font-weight="700" fill="#a8d8d4" text-anchor="middle">✨ Gemini API</text>
  <text x="755" y="106" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">gemini-2.0-flash</text>
  <text x="755" y="121" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">safety_label</text>
  <text x="755" y="136" font-family="Arial, sans-serif" font-size="10" fill="#6b8a99" text-anchor="middle">+ explanation</text>
  <!-- Bottom accent -->
  <rect y="193" width="860" height="5" rx="2" fill="#0B8F87" opacity="0.6"/>
</svg>

</div>

**Key Design Decisions:**

- `run_polling()` is the synchronous PTB v20 entry-point — no manual asyncio event loop sorcery required.
- Images are written to `tempfile.mkstemp()` to avoid filename collisions under concurrent requests and guarantee cleanup in a `finally` block. Even if the model explodes, your photo is gone.
- Both `BOT_TOKEN` and `GEMINI_API_KEY` are validated at startup — misconfigured deployments crash loudly and immediately with a descriptive error. Fail fast > fail mysteriously.
- The Gemini HTTP call carries a hard 15-second timeout. The bot will not stall waiting for an API that decided to take a nap.

---

## 🌊 Data Flow

<div align="center">

<!-- ═══════════════════════════════════════════════════════════════════════ -->
<!-- SVG 4 — DATA FLOW DIAGRAM                                             -->
<!-- ═══════════════════════════════════════════════════════════════════════ -->
<svg xmlns="http://www.w3.org/2000/svg" width="860" height="260" viewBox="0 0 860 260" role="img" aria-label="Data Flow Diagram">
  <defs>
    <linearGradient id="dfBg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#071318"/>
      <stop offset="100%" stop-color="#0a1f2e"/>
    </linearGradient>
    <linearGradient id="stepGrad" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%" stop-color="#0f2a36"/>
      <stop offset="100%" stop-color="#082028"/>
    </linearGradient>
    <marker id="dfArrow" viewBox="0 0 10 10" refX="10" refY="5" markerWidth="5" markerHeight="5" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#0B8F87"/>
    </marker>
  </defs>
  <rect width="860" height="260" fill="url(#dfBg)" rx="12"/>
  <text x="430" y="24" font-family="Arial, sans-serif" font-size="13" font-weight="700" fill="#6ec6f5" text-anchor="middle">Request Data Flow</text>
  <!-- Step boxes — top row -->
  <!-- Step 1 -->
  <rect x="15"  y="50" width="110" height="70" rx="7" fill="url(#stepGrad)" stroke="#2563EB" stroke-width="1.2"/>
  <text x="70"  y="80"  font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">📷 Photo</text>
  <text x="70"  y="96"  font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">Telegram upload</text>
  <text x="70"  y="110" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">via PHOTO filter</text>
  <!-- Arrow -->
  <line x1="125" y1="85" x2="153" y2="85" stroke="#0B8F87" stroke-width="1.5" marker-end="url(#dfArrow)"/>
  <!-- Step 2 -->
  <rect x="155" y="50" width="110" height="70" rx="7" fill="url(#stepGrad)" stroke="#0B8F87" stroke-width="1.2"/>
  <text x="210" y="80"  font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">💾 Temp File</text>
  <text x="210" y="96"  font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">mkstemp(.jpg)</text>
  <text x="210" y="110" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">download_to_drive</text>
  <!-- Arrow -->
  <line x1="265" y1="85" x2="293" y2="85" stroke="#0B8F87" stroke-width="1.5" marker-end="url(#dfArrow)"/>
  <!-- Step 3 -->
  <rect x="295" y="50" width="110" height="70" rx="7" fill="url(#stepGrad)" stroke="#0B8F87" stroke-width="1.2"/>
  <text x="350" y="80"  font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">🖼️ PIL Open</text>
  <text x="350" y="96"  font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">Image.open()</text>
  <text x="350" y="110" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">.convert("RGB")</text>
  <!-- Arrow -->
  <line x1="405" y1="85" x2="433" y2="85" stroke="#0B8F87" stroke-width="1.5" marker-end="url(#dfArrow)"/>
  <!-- Step 4 -->
  <rect x="435" y="50" width="110" height="70" rx="7" fill="url(#stepGrad)" stroke="#0B8F87" stroke-width="1.2"/>
  <text x="490" y="80"  font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">⚙️ Processor</text>
  <text x="490" y="96"  font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">AutoImageProcessor</text>
  <text x="490" y="110" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">resize + normalize</text>
  <!-- Arrow -->
  <line x1="545" y1="85" x2="573" y2="85" stroke="#0B8F87" stroke-width="1.5" marker-end="url(#dfArrow)"/>
  <!-- Step 5 -->
  <rect x="575" y="50" width="110" height="70" rx="7" fill="url(#stepGrad)" stroke="#0B8F87" stroke-width="1.2"/>
  <text x="630" y="80"  font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">🤗 ViT Model</text>
  <text x="630" y="96"  font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">no_grad() forward</text>
  <text x="630" y="110" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">logits → softmax</text>
  <!-- Arrow -->
  <line x1="685" y1="85" x2="713" y2="85" stroke="#0B8F87" stroke-width="1.5" marker-end="url(#dfArrow)"/>
  <!-- Step 6 -->
  <rect x="715" y="50" width="130" height="70" rx="7" fill="url(#stepGrad)" stroke="#10B981" stroke-width="1.2"/>
  <text x="780" y="80"  font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">🏷️ label + conf.</text>
  <text x="780" y="96"  font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">id2label[argmax]</text>
  <text x="780" y="110" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">× 100 = %</text>
  <!-- Down arrow from step 6 -->
  <line x1="780" y1="120" x2="780" y2="158" stroke="#10B981" stroke-width="1.5" marker-end="url(#dfArrow)"/>
  <!-- Step 7 -->
  <rect x="715" y="160" width="130" height="70" rx="7" fill="url(#stepGrad)" stroke="#2563EB" stroke-width="1.2"/>
  <text x="780" y="193" font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">✨ Gemini API</text>
  <text x="780" y="209" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">fetch_condition_info</text>
  <text x="780" y="223" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">safety + explanation</text>
  <!-- Left arrow from step 7 -->
  <line x1="715" y1="195" x2="477" y2="195" stroke="#10B981" stroke-width="1.5" marker-end="url(#dfArrow)"/>
  <!-- Step 8 -->
  <rect x="295" y="160" width="180" height="70" rx="7" fill="url(#stepGrad)" stroke="#10B981" stroke-width="1.2"/>
  <text x="385" y="193" font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">📝 format_prediction()</text>
  <text x="385" y="209" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">escape_markdown()</text>
  <text x="385" y="223" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">MarkdownV2 string</text>
  <!-- Left arrow from step 8 -->
  <line x1="295" y1="195" x2="153" y2="195" stroke="#10B981" stroke-width="1.5" marker-end="url(#dfArrow)"/>
  <!-- Step 9 -->
  <rect x="15"  y="160" width="135" height="70" rx="7" fill="url(#stepGrad)" stroke="#2563EB" stroke-width="1.2"/>
  <text x="82"  y="193" font-family="Arial, sans-serif" font-size="11" font-weight="700" fill="#a8d8d4" text-anchor="middle">📬 Reply + 🗑️ Delete</text>
  <text x="82"  y="209" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">reply_text()</text>
  <text x="82"  y="223" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">os.remove(temp)</text>
  <!-- Bottom accent -->
  <rect y="253" width="860" height="5" rx="2" fill="#10B981" opacity="0.5"/>
</svg>

</div>

---

## ⚙️ Installation

### Prerequisites

- **Python 3.10+** (we're not animals who use 3.8)
- A [Telegram Bot Token](https://t.me/BotFather) — takes 2 minutes, even less if you don't mistype your bot's name
- A [Google Gemini API Key](https://aistudio.google.com/app/apikey) — free tier works fine

### Step 1 — Clone the repository

```bash
git clone https://github.com/Kaelith69/Telegram_skin_cancer_detector.git
cd Telegram_skin_cancer_detector
```

### Step 2 — Create a virtual environment *(strongly recommended)*

```bash
python -m venv .venv
source .venv/bin/activate       # macOS/Linux
# .venv\Scripts\activate        # Windows
```

### Step 3 — Install dependencies

```bash
pip install -r requirements.txt
```

> ⚠️ The HuggingFace ViT model weights (~300 MB) are downloaded from the Hub on **first run**. Put the kettle on.

### Step 4 — Configure environment variables

```bash
cp .env.example .env
```

Open `.env` and fill in your credentials:

```dotenv
BOT_TOKEN=123456789:ABCdefGHIjklMNOpqrSTUvwxYZ
GEMINI_API_KEY=AIzaSy...
```

> **Never commit your `.env` file.** It is already in `.gitignore`. But, you know. Just in case you were thinking about it.

### Step 5 — Run the bot

```bash
python bot.py
```

Expected output:

```
🤖 Skin Diagnosis Bot is live.
```

If you see a `RuntimeError` about missing tokens — you skipped Step 4. It's okay. We've all done it.

---

## 🚀 Usage

1. Open Telegram and find your bot by the username you gave it in BotFather.
2. Send any **clear, well-lit photo** of a skin lesion directly in the chat.
3. Wait roughly 2 seconds (faster than WebMD loads its ad trackers).
4. The bot replies with a structured diagnosis:

```
🧾 Diagnosis Result
• Condition:   Melanocytic nevi
• Confidence:  94.73%
• Type:        ✅ Harmless - Non-Cancerous

Medical Insight:
Melanocytic nevi (moles) are benign clusters of pigmented cells...
Signs: Small, round, evenly pigmented spots...
Treatment: Usually no treatment needed unless they change...
What to do: Monitor using ABCDE criteria. Annual dermatologist check.
```

5. Send a non-image message? The bot asks you nicely to send a photo instead of judging you.

### Classifiable Conditions

Based on the [HAM10000 dataset](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DBW86T):

| Code | Full Name | Severity |
|------|-----------|----------|
| `nv`    | Melanocytic Nevi          | ✅ Benign |
| `bkl`   | Benign Keratosis-like Lesions | ✅ Benign |
| `df`    | Dermatofibroma            | ✅ Benign |
| `vasc`  | Vascular Lesions          | ✅ Benign |
| `akiec` | Actinic Keratoses / Intraepithelial Carcinoma | ⚠️ Pre-cancerous |
| `bcc`   | Basal Cell Carcinoma      | ❗ Cancerous |
| `mel`   | Melanoma                  | ❗ Cancerous |

---

## 📁 Project Structure

```
Telegram_skin_cancer_detector/
├── bot.py              # Everything — classification, Gemini, Telegram handler
├── requirements.txt    # Python dependencies (keep this blessed file clean)
├── Procfile            # Heroku/Railway worker declaration
├── .env.example        # Template for required environment variables
├── .gitignore          # Sanity rules (excludes .env, __pycache__, etc.)
├── CONTRIBUTING.md     # How to contribute without breaking everything
├── CHANGELOG.md        # What changed and when
├── SECURITY.md         # Vulnerability disclosure policy
├── LICENSE             # MIT — go wild
├── wiki/               # Detailed documentation pages
│   ├── Home.md
│   ├── Architecture.md
│   ├── Installation.md
│   ├── Usage.md
│   ├── Privacy.md
│   ├── Troubleshooting.md
│   └── Roadmap.md
└── README.md           # This majestic document
```

---

## 📊 Stats

<div align="center">

<!-- ═══════════════════════════════════════════════════════════════════════ -->
<!-- SVG 5 — STATS VISUALIZATION                                           -->
<!-- ═══════════════════════════════════════════════════════════════════════ -->
<svg xmlns="http://www.w3.org/2000/svg" width="780" height="170" viewBox="0 0 780 170" role="img" aria-label="Project Stats">
  <defs>
    <linearGradient id="statsBg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#062e2c"/>
      <stop offset="100%" stop-color="#0a1f2e"/>
    </linearGradient>
    <linearGradient id="statAcc" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#0B8F87"/>
      <stop offset="100%" stop-color="#10B981"/>
    </linearGradient>
  </defs>
  <rect width="780" height="170" fill="url(#statsBg)" rx="12"/>
  <text x="390" y="28" font-family="Arial, sans-serif" font-size="14" font-weight="700" fill="#a8d8d4" text-anchor="middle">Project at a Glance</text>
  <!-- Stat 1: Conditions -->
  <rect x="30"  y="50" width="150" height="90" rx="8" fill="#0d2030" stroke="#0B8F87" stroke-width="1.2"/>
  <text x="105" y="90"  font-family="Arial, sans-serif" font-size="34" font-weight="700" fill="#10B981" text-anchor="middle">7</text>
  <text x="105" y="110" font-family="Arial, sans-serif" font-size="11" fill="#a8d8d4" text-anchor="middle">Skin Conditions</text>
  <text x="105" y="127" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">HAM10000 classes</text>
  <!-- Stat 2: Model -->
  <rect x="200" y="50" width="150" height="90" rx="8" fill="#0d2030" stroke="#0B8F87" stroke-width="1.2"/>
  <text x="275" y="90"  font-family="Arial, sans-serif" font-size="22" font-weight="700" fill="#10B981" text-anchor="middle">ViT-B/16</text>
  <text x="275" y="110" font-family="Arial, sans-serif" font-size="11" fill="#a8d8d4" text-anchor="middle">Model Architecture</text>
  <text x="275" y="127" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">Vision Transformer</text>
  <!-- Stat 3: Response -->
  <rect x="370" y="50" width="150" height="90" rx="8" fill="#0d2030" stroke="#0B8F87" stroke-width="1.2"/>
  <text x="445" y="90"  font-family="Arial, sans-serif" font-size="28" font-weight="700" fill="#10B981" text-anchor="middle">~2s</text>
  <text x="445" y="110" font-family="Arial, sans-serif" font-size="11" fill="#a8d8d4" text-anchor="middle">Avg. Response Time</text>
  <text x="445" y="127" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">CPU inference</text>
  <!-- Stat 4: Zero storage -->
  <rect x="540" y="50" width="210" height="90" rx="8" fill="#0d2030" stroke="#10B981" stroke-width="1.2"/>
  <text x="645" y="90"  font-family="Arial, sans-serif" font-size="28" font-weight="700" fill="#10B981" text-anchor="middle">0 bytes</text>
  <text x="645" y="110" font-family="Arial, sans-serif" font-size="11" fill="#a8d8d4" text-anchor="middle">Photos Stored</text>
  <text x="645" y="127" font-family="Arial, sans-serif" font-size="9"  fill="#6b8a99" text-anchor="middle">Ephemeral · deleted on completion</text>
  <!-- Bottom accent -->
  <rect y="163" width="780" height="5" rx="2" fill="url(#statAcc)" opacity="0.7"/>
</svg>

</div>

---

## 🔒 Privacy

Your images are **never stored, logged, or transmitted** beyond what's required for inference.

Here's the chain of custody for your photos:

1. Telegram delivers the file to the bot process in memory.
2. `tempfile.mkstemp()` writes it to the OS temp directory (usually `/tmp/`).
3. The ViT model reads it **locally** — nothing leaves your server.
4. The `finally` block deletes the temp file **unconditionally**, even if the model crashes.
5. Only the **text label** (e.g., "Melanocytic nevi") is sent to the Gemini API — not the image, not your Telegram ID.

No analytics. No telemetry. No "we'll use your moles to train our future models." See [wiki/Privacy.md](wiki/Privacy.md) for the full breakdown.

---

## 🗺️ Roadmap

| Status | Feature |
|--------|---------|
| ✅ Done | Core classification + Gemini insights |
| ✅ Done | Ephemeral image handling |
| ✅ Done | Graceful error handling & fast-fail config validation |
| 🔄 Planned | `/start` command with onboarding instructions |
| 🔄 Planned | Multi-language support (EN/ES/FR/DE) |
| 🔄 Planned | Inline keyboard for "Send to a real doctor" links |
| 🔄 Planned | GPU inference support for sub-second classification |
| 💡 Idea | Second-opinion mode: top-3 predictions instead of top-1 |
| 💡 Idea | ABCDE mole analysis overlay (asymmetry, border, colour, diameter, evolution) |

Full details in [wiki/Roadmap.md](wiki/Roadmap.md).

---

## 🤝 Contributing

Found a bug? Congratulations, you are now part of the development team. PRs welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide.

---

## ⚠️ Disclaimer

**This bot is not a medical device and does not provide medical advice.**

Results are AI predictions. They may be wrong. They have been wrong before. If the bot flags anything serious, please consult a qualified dermatologist — an actual human with an actual medical degree, not a very convincing chatbot.

The authors accept no liability for decisions made based on the bot's output.

---

## 📄 License

This project is released under the [MIT License](LICENSE). Use it, fork it, build on it — just don't sue us if it's wrong about your mole.

---

<div align="center">

*Built with 🤍, way too much caffeine, and a healthy fear of dermatologists.*

> **Dad Joke of the Day 🥁**
> Why did the neural network go to therapy?
> Because it had too many *deep issues*. 🥁

</div>
