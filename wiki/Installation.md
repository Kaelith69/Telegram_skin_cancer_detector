# ⚙️ Installation

This page covers every step to go from "I just cloned the repo" to "the bot is live and classifying skin lesions."

---

## Prerequisites

Before you start, make sure you have:

| Requirement | Notes |
|-------------|-------|
| **Python 3.10+** | Check with `python3 --version`. 3.8 might work. We haven't tested 3.8. Use 3.10+. |
| **pip** | Comes with Python. If it doesn't, you have bigger problems. |
| **Telegram Bot Token** | Get one from [@BotFather](https://t.me/BotFather) in about 2 minutes. |
| **Google Gemini API Key** | Get one from [Google AI Studio](https://aistudio.google.com/app/apikey). Free tier works fine. |
| **~500 MB disk space** | For the ViT model weights (~300 MB) + Python packages (~150 MB). |
| **Internet connection** | First run downloads the model from HuggingFace Hub. Subsequent runs use the cached weights. |

---

## Step 1 — Clone the Repository

```bash
git clone https://github.com/Kaelith69/Telegram_skin_cancer_detector.git
cd Telegram_skin_cancer_detector
```

---

## Step 2 — Create a Virtual Environment

Strongly recommended. This keeps the project's dependencies isolated from your system Python.

**macOS / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows (Command Prompt):**
```cmd
python -m venv .venv
.venv\Scripts\activate.bat
```

**Windows (PowerShell):**
```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

You should see `(.venv)` in your prompt when the environment is active.

---

## Step 3 — Install Dependencies

```bash
pip install -r requirements.txt
```

This installs:
- `python-telegram-bot==20.7`
- `transformers` (HuggingFace)
- `torch` (PyTorch)
- `Pillow`
- `requests`
- `python-dotenv`
- `nest_asyncio`

> ⏳ **PyTorch is large.** The first install can take 3–10 minutes depending on your connection. This is normal. Stare out a window for a bit.

---

## Step 4 — Get Your API Credentials

### Telegram Bot Token

1. Open Telegram and search for [@BotFather](https://t.me/BotFather).
2. Send `/newbot`.
3. Follow the prompts to set a name and username for your bot.
4. BotFather will give you a token like: `123456789:ABCdefGHIjklMNOpqrSTUvwxYZ`
5. Copy it. Guard it with your life (or at least your `.gitignore`).

### Google Gemini API Key

1. Visit [https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey).
2. Sign in with your Google account.
3. Click **Create API key**.
4. Copy the key (starts with `AIzaSy...`).

---

## Step 5 — Configure Environment Variables

```bash
cp .env.example .env
```

Open `.env` with your favourite editor and fill in your credentials:

```dotenv
BOT_TOKEN=123456789:ABCdefGHIjklMNOpqrSTUvwxYZ
GEMINI_API_KEY=AIzaSy...
```

**Important:**
- Do **not** put quotes around the values.
- Do **not** commit this file. It is already in `.gitignore`. If you've already committed it by accident, rotate your tokens immediately and consider them compromised.

---

## Step 6 — Run the Bot

```bash
python bot.py
```

On first run, the HuggingFace model weights are downloaded and cached. You'll see progress output from the Transformers library. This only happens once.

When the bot is ready, you'll see:

```
🤖 Skin Diagnosis Bot is live.
```

---

## Step 7 — Test It

1. Open Telegram.
2. Search for your bot by the username you gave it in BotFather.
3. Send a photo of a skin lesion (or any image, to test the pipeline).
4. The bot should reply within ~2 seconds.

---

## Deployment Options

### Heroku

```bash
heroku create your-app-name
heroku config:set BOT_TOKEN=your_token
heroku config:set GEMINI_API_KEY=your_key
git push heroku main
heroku ps:scale worker=1
```

The `Procfile` is already configured:
```
worker: python bot.py
```

> ⚠️ Heroku's free tier was discontinued in November 2022. You will need a paid Eco or Basic dyno. The model weights (~300 MB) are cached during the dyno's lifetime but are lost on restarts — the first message after a restart will trigger a re-download.

### Railway

1. Connect your GitHub repo to Railway.
2. Set `BOT_TOKEN` and `GEMINI_API_KEY` in the Environment settings.
3. Railway will detect the `Procfile` and run the worker process.

### VPS / Self-Hosted

Run the bot in a persistent screen or tmux session, or create a systemd service:

```ini
[Unit]
Description=Skin Cancer Detector Telegram Bot
After=network.target

[Service]
User=youruser
WorkingDirectory=/path/to/Telegram_skin_cancer_detector
EnvironmentFile=/path/to/.env
ExecStart=/path/to/.venv/bin/python bot.py
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

---

## Troubleshooting Installation

See [Troubleshooting](Troubleshooting.md) for common issues. Short version:

- `RuntimeError: BOT_TOKEN environment variable is not set.` → You forgot to fill in `.env`.
- `ModuleNotFoundError` → Your virtualenv isn't active, or you ran pip outside it.
- `OSError: [WinError 32]` during image processing → Windows file lock issue. Fixed in v1.1.0 with the `os.close(fd)` call.
