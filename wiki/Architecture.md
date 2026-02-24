# 🏗️ Architecture

This page explains how the Skin Cancer Detector Bot is structured, why key design decisions were made, and what each component does.

---

## High-Level Overview

The bot is a single-file Python application (`bot.py`) that wires together four external systems:

```
Telegram User
    │  [sends photo]
    ▼
python-telegram-bot v20
    │  ApplicationBuilder → run_polling()
    │  MessageHandler(filters.PHOTO) → classify_image()
    ▼
HuggingFace Transformers (local inference)
    │  AutoImageProcessor (resize, normalize)
    │  AutoModelForImageClassification (ViT)
    │  torch.no_grad() → softmax → label + confidence
    ▼
Google Gemini 2.0 Flash API
    │  POST /v1beta/models/gemini-2.0-flash:generateContent
    │  prompt: "Is '{label}' cancerous...?"
    │  → safety_label + explanation
    ▼
format_prediction() → escape_markdown() → reply_text(MarkdownV2)
```

---

## Component Breakdown

### 1. Bot Framework — `python-telegram-bot` v20

The application is built on `python-telegram-bot` v20 using the `ApplicationBuilder` pattern and `run_polling()`. This provides:

- **Polling-based updates** — the bot continuously asks Telegram for new messages. No inbound port, no webhook server, no SSL certificate to manage.
- **Async handlers** — `classify_image` is an `async` function, allowing the bot to handle multiple messages without blocking. (In practice, the ViT inference is CPU-bound and synchronous, but the framework is ready for async I/O.)
- **Message filtering** — `MessageHandler(filters.PHOTO, classify_image)` ensures only photo messages trigger inference. Text messages receive a polite redirect.

### 2. Image Preprocessing — Pillow + AutoImageProcessor

When a photo arrives:

```python
fd, image_path = tempfile.mkstemp(suffix=".jpg")
os.close(fd)
await photo.download_to_drive(image_path)
image = Image.open(image_path).convert("RGB")
inputs = processor(images=image, return_tensors="pt")
```

- `tempfile.mkstemp()` creates a uniquely named temp file and returns an open file descriptor. The descriptor is immediately closed so `download_to_drive` can write to the path.
- Pillow's `Image.open(...).convert("RGB")` normalises image mode (handles PNG, WEBP, grayscale, etc.).
- `AutoImageProcessor` handles all the ViT-specific preprocessing: resizing to 224×224, normalising pixel values to `[0, 1]`, converting to a PyTorch tensor.

### 3. Classification — HuggingFace ViT

```python
with torch.no_grad():
    outputs = model(**inputs)
    probs = torch.nn.functional.softmax(outputs.logits, dim=1)[0]

predicted_idx = torch.argmax(probs).item()
label = model.config.id2label[predicted_idx]
confidence = probs[predicted_idx].item() * 100
```

- **Model:** `Anwarkh1/Skin_Cancer-Image_Classification` — a Vision Transformer (ViT-B/16) fine-tuned on the HAM10000 dataset.
- **`torch.no_grad()`** disables gradient tracking, significantly reducing memory usage and inference time for CPU-only deployment.
- **Softmax** converts raw logits into a probability distribution across 7 condition classes.
- **`id2label`** maps the predicted class index back to a human-readable condition name from the model's config.

### 4. Medical Insights — Google Gemini 2.0 Flash

```python
url = f"https://generativelanguage.googleapis.com/.../gemini-2.0-flash:generateContent?key={GEMINI_API_KEY}"
prompt = f"Is '{condition}' cancerous or non-cancerous? ..."
response = requests.post(url, headers=headers, json=data, timeout=15)
```

- Only the **text label** (e.g., `"Melanocytic nevi"`) is sent to Gemini. **No image data leaves the server.**
- The prompt instructs Gemini to return a structured response: severity label on line 1, explanation on lines 2+.
- A **15-second timeout** prevents the bot from stalling if the API is slow or unavailable.
- On any exception (network error, API error, parse failure), the function returns a safe fallback instead of crashing.

### 5. Response Formatting

```python
def format_prediction(label, confidence, safety, info):
    # escape_markdown() prevents MarkdownV2 parse errors
    return f"🧾 *Diagnosis Result*\n• *Condition:* {label_esc}\n..."
```

`escape_markdown()` uses a regex to escape all MarkdownV2 special characters (`_*[]()~\`>#+-=|{}.!`). This is critical because condition names like `"Actinic Keratoses / Intraepithelial Carcinoma"` contain characters that would break Telegram's MarkdownV2 renderer.

---

## Design Decisions

| Decision | Rationale |
|----------|-----------|
| Single file (`bot.py`) | Keeps the project simple and easy to deploy. Refactor when complexity demands it. |
| `run_polling()` not webhooks | No web server infrastructure needed. Simpler deployment, fewer attack vectors. |
| `tempfile.mkstemp()` | Avoids filename collisions under concurrent requests; guarantees unique temp paths. |
| `os.close(fd)` immediately | Prevents `PermissionError` on Windows where the FD must be closed before another process can write to the path. |
| Config validation at startup | Fail fast with a clear error message rather than failing silently or mid-request. |
| CPU-only inference | Keeps deployment requirements minimal. No GPU needed, works on free-tier Heroku dynos. |
| 15s Gemini timeout | Prevents indefinite hangs. Users get a fallback message rather than waiting forever. |
| Only label sent to Gemini | Preserves user privacy — no image data, no Telegram user ID, no metadata. |

---

## Dependency Map

```
bot.py
├── python-telegram-bot  — bot lifecycle, message handling, file download
├── transformers         — ViT model loading and image processing
├── torch                — model inference backend
├── Pillow               — image decoding and RGB conversion
├── requests             — Gemini API HTTP calls
└── python-dotenv        — .env file loading
```

---

## Deployment Architecture

The `Procfile` declares:

```
worker: python bot.py
```

This makes the bot deployable as a **worker dyno** on Heroku or a **worker process** on Railway — no web process, no port binding, just the polling loop.

For production deployments:
- Set `BOT_TOKEN` and `GEMINI_API_KEY` as environment variables in your platform's dashboard.
- The model weights (~300 MB) are downloaded from HuggingFace Hub on first startup and cached locally. Ephemeral platforms (like Heroku free tier) will re-download on each restart.
