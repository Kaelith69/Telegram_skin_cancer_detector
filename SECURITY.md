# 🔐 Security Policy

## Supported Versions

| Version | Supported          |
|---------|--------------------|
| 1.1.x   | ✅ Yes             |
| 1.0.x   | ⚠️ Best-effort     |
| < 1.0   | ❌ No              |

---

## Reporting a Vulnerability

If you discover a security vulnerability in this project, please **do not open a public GitHub issue**. Responsible disclosure matters — even for a Telegram bot.

### How to report

1. **Email:** Open a [GitHub Security Advisory](https://github.com/Kaelith69/Telegram_skin_cancer_detector/security/advisories/new) using GitHub's private vulnerability reporting feature. This keeps the disclosure private until a fix is in place.

2. **Include in your report:**
   - A description of the vulnerability and its potential impact
   - Steps to reproduce (proof-of-concept if possible)
   - The version(s) affected
   - Any suggested fix or mitigation you've identified

3. **What to expect:**
   - Acknowledgement within **72 hours**
   - A status update within **7 days**
   - Coordinated disclosure once a fix is available

---

## Scope

### In scope

| Area | Examples |
|------|---------|
| **Credential exposure** | Bot token or API key leaked via logs, error messages, or HTTP responses |
| **Image data leakage** | User images persisted beyond the `finally` cleanup block |
| **Injection vulnerabilities** | Malformed image/text inputs causing unexpected code execution |
| **Dependency vulnerabilities** | Known CVEs in `transformers`, `python-telegram-bot`, `Pillow`, or `requests` |
| **API key forwarding** | Gemini API key accidentally included in outbound request headers or logs |

### Out of scope

- Issues requiring physical access to the host server
- Rate-limit abuse (this is a polling bot with no auth layer — by design)
- Theoretical vulnerabilities with no demonstrated impact
- Vulnerabilities in third-party services (HuggingFace Hub, Google Gemini, Telegram servers)

---

## Security Design Notes

This bot was designed with a minimal attack surface:

- **No persistent storage.** Images are written to `tempfile.mkstemp()` and deleted in a `finally` block. There is no database, no image store, and no user profiles.
- **No web server.** The bot uses polling (`run_polling()`), not webhooks. There is no inbound HTTP port to attack.
- **Credential isolation.** `BOT_TOKEN` and `GEMINI_API_KEY` are loaded from environment variables at startup. They are never printed to stdout, included in responses, or written to disk.
- **Only the label text is sent to Gemini.** The raw image is never forwarded to any external API. Gemini only receives the text label (e.g., `"Melanocytic nevi"`).
- **Timeout enforced.** The Gemini HTTP call has a 15-second timeout to prevent the bot from being stalled by a slow or malicious API response.

---

## Dependencies

Please report vulnerabilities in the following direct dependencies through the process above:

| Package | Purpose |
|---------|---------|
| `python-telegram-bot==20.7` | Telegram Bot API wrapper |
| `transformers` | HuggingFace ViT model loading |
| `torch` | PyTorch inference backend |
| `Pillow` | Image decoding and preprocessing |
| `requests` | Gemini API HTTP calls |
| `python-dotenv` | Environment variable loading |

---

## Disclosure Policy

We follow a **coordinated disclosure** model. We will:

1. Acknowledge the report privately.
2. Investigate and develop a fix.
3. Release the fix.
4. Credit the reporter (unless they prefer to remain anonymous) in the `CHANGELOG.md`.

We will not pursue legal action against researchers who follow this policy and act in good faith.

---

*Thank you for helping keep this project — and its users — safe.*
