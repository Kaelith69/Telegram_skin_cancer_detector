# 🔧 Troubleshooting

Something broke. This happens. Here are the most common issues and how to fix them.

---

## Startup Errors

### `RuntimeError: BOT_TOKEN environment variable is not set.`

**What happened:** You didn't configure your `.env` file, or the bot can't find it.

**Fix:**
```bash
# Make sure .env exists
ls -la .env

# If it doesn't:
cp .env.example .env
# Then open .env and fill in BOT_TOKEN and GEMINI_API_KEY
```

Also check that you're running the bot from the project root directory (where `.env` lives), not from a subdirectory.

---

### `RuntimeError: GEMINI_API_KEY environment variable is not set.`

Same as above, but for your Gemini key. Open `.env` and make sure `GEMINI_API_KEY` is set.

---

### `ModuleNotFoundError: No module named 'telegram'` (or any other module)

**What happened:** Your virtual environment is not active, or you installed the dependencies outside it.

**Fix:**
```bash
# Activate the virtualenv
source .venv/bin/activate       # macOS/Linux
# .venv\Scripts\activate         # Windows

# Then install dependencies
pip install -r requirements.txt
```

---

## Model Loading Errors

### Hangs on startup with no output

**What happened:** The HuggingFace model is downloading for the first time (~300 MB). This is normal.

**Fix:** Wait. It only happens once. Subsequent startups use the cached weights.

If it hangs indefinitely with no progress:
- Check your internet connection
- Check if HuggingFace Hub is reachable: `curl -I https://huggingface.co`
- Try setting `HF_HUB_OFFLINE=0` and running again

---

### `OSError: We couldn't connect to 'https://huggingface.co'`

**What happened:** No internet connection, or HuggingFace Hub is blocked.

**Fix:**
- Check your connection
- If running in a restricted network, you may need to download the model manually and set `HF_HUB_OFFLINE=1` with the model cached

---

## Image Processing Errors

### `OSError: [WinError 32] The process cannot access the file because it is being used by another process`

**What happened:** On Windows, the file descriptor from `tempfile.mkstemp()` was still open when `download_to_drive` tried to write to it.

**Status:** Fixed in v1.1.0 by adding `os.close(fd)` immediately after `mkstemp()`.

**Fix:** Update to the latest version. If you're on v1.1.0+ and still seeing this, check whether your antivirus is locking the temp file.

---

### `PIL.UnidentifiedImageError: cannot identify image file`

**What happened:** The downloaded file is not a valid image, or was corrupted during download.

**Fix:** Ask the user to resend the image. This is typically a transient Telegram API issue.

---

### `⚠️ An error occurred while processing the image.`

This is the bot's catch-all error message. Check the bot's console output for the specific exception:

```
[ERROR] <specific exception here>
```

Common causes:
- Corrupted image download → resend the photo
- Model inference failed → check `torch` installation
- Gemini API error → see below

---

## Gemini API Errors

### `requests.exceptions.Timeout`

**What happened:** The Gemini API didn't respond within 15 seconds.

**Fix:** This is handled gracefully — the bot returns a fallback message. The user won't see a crash. If it's happening frequently, check your network connectivity to `generativelanguage.googleapis.com`.

---

### `requests.exceptions.HTTPError: 400 Client Error`

**What happened:** The Gemini API rejected the request. Usually a malformed prompt or an invalid API key.

**Fix:**
1. Verify your `GEMINI_API_KEY` is correct and active in [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Check if you've exceeded your free-tier quota

---

### `requests.exceptions.HTTPError: 429 Too Many Requests`

**What happened:** You've hit the Gemini API rate limit.

**Fix:** The free tier has request-per-minute and requests-per-day limits. If the bot is busy, add a short delay or upgrade your Google AI plan.

---

## Telegram API Errors

### Bot is not responding to messages

**Checklist:**
1. Is `python bot.py` still running? (Check your terminal)
2. Is the polling loop active? You should see no errors after `🤖 Skin Diagnosis Bot is live.`
3. Did you send a photo (not a file or document)? The bot only handles `filters.PHOTO`
4. Is your `BOT_TOKEN` correct? Test it:
   ```bash
   curl "https://api.telegram.org/bot<YOUR_TOKEN>/getMe"
   ```
   Should return your bot's info as JSON.

---

### `telegram.error.Conflict: terminated by other getUpdates request`

**What happened:** Two instances of the bot are running simultaneously, both polling the same token.

**Fix:** Stop all running instances and restart just one:
```bash
pkill -f "python bot.py"   # or Task Manager on Windows
python bot.py
```

---

## General Debugging Tips

Enable verbose output by adding print statements or checking the console:

- The bot logs `[EVENT] Received photo: message_id=...` when a photo arrives
- The bot logs `[EVENT] Diagnosis: <label> (<confidence>%)` after classification
- The bot logs `[ERROR] ...` for any caught exception

If you're stuck, [open an issue](https://github.com/Kaelith69/Telegram_skin_cancer_detector/issues) with:
- The full error message / stack trace
- Your Python version (`python3 --version`)
- Your OS
- Whether you're running locally or on a hosted platform
