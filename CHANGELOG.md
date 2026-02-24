# 📋 Changelog

All notable changes to this project are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.1.0] — 2024-12-01

### Added
- **Gemini 2.0 Flash integration** — the bot now explains *what the condition is*, not just *what it's called*. Because "Actinic Keratoses" is not a useful answer to anyone outside a medical school.
- **Safety severity label** — one-line verdict: Harmless / Pre-cancerous / Cancerous, surfaced at the top of every reply. Skip to the important bit.
- **Startup validation** — bot now crashes loudly and immediately if `BOT_TOKEN` or `GEMINI_API_KEY` are not set. No more silent ghost bots that just don't respond to anything.
- **15-second Gemini timeout** — because waiting forever for an API response is not a feature.
- **MarkdownV2 escape helper** — `escape_markdown()` prevents any condition name with special characters from exploding the Telegram message renderer. Looking at you, `Actinic Keratoses / Intraepithelial Carcinoma`.
- **`Procfile`** — Heroku/Railway deployment support.

### Changed
- Response format now uses `format_prediction()` for consistent, structured output.
- Temporary image download now uses `tempfile.mkstemp()` instead of a fixed path — no more race conditions when two users send photos at the same time.

### Fixed
- Fixed bug where the OS file descriptor from `mkstemp` was left open, causing `PermissionError` on Windows during download. Added `os.close(fd)` immediately after `mkstemp()`. (Windows users: you're welcome.)

---

## [1.0.0] — 2024-10-15

### Added
- Initial release.
- **HuggingFace ViT model** (`Anwarkh1/Skin_Cancer-Image_Classification`) for classifying skin lesions into 7 HAM10000 categories.
- **Telegram bot handler** using `python-telegram-bot` v20 with `ApplicationBuilder` and `run_polling()`.
- **Confidence score** output (softmax probability × 100).
- **Ephemeral image processing** — photos downloaded to OS temp dir, deleted after analysis in a `finally` block.
- **Graceful error handling** — network errors, model errors, and missing photos all return user-friendly messages instead of stack traces.
- **Environment variable configuration** via `python-dotenv` with `.env.example` template.

---

> *"The first release was written at 2 AM and somehow worked on the first try. We don't talk about what was sacrificed to make that happen."*
