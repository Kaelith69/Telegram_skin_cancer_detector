# 🗺️ Roadmap

This page outlines where the project is heading and what's being considered for future versions.

---

## Current Status: v1.1.0

The bot is functional and actively used. Core features are stable:
- ✅ 7-class skin lesion classification (HAM10000 ViT model)
- ✅ Confidence scores
- ✅ Gemini 2.0 Flash medical insights
- ✅ Safety severity labels
- ✅ Ephemeral image processing (zero data retention)
- ✅ Graceful error handling
- ✅ Heroku/Railway deployment support

---

## Planned Features

### v1.2.0 — User Experience

| Feature | Priority | Notes |
|---------|----------|-------|
| `/start` command with onboarding message | High | Explain what the bot does and its limitations before the user sends their first photo |
| `/help` command | Medium | List available commands and usage tips |
| Inline keyboard with "Learn More" links | Medium | Button linking to [Usage](Usage.md), disclaimer, and dermatologist-finder resources |
| Multi-language support | Medium | EN/ES/FR/DE — bot UI and Gemini prompt translations |

---

### v1.3.0 — Classification Improvements

| Feature | Priority | Notes |
|---------|----------|-------|
| Top-3 predictions | High | Show top 3 condition predictions with individual confidence scores instead of just the top-1. Useful when the model is uncertain. |
| Uncertainty threshold | Medium | If top-1 confidence is below a threshold (e.g., <60%), warn the user that the model is not confident and to prioritise medical advice |
| Image quality check | Medium | Reject very blurry, low-resolution, or pitch-dark images before inference with a helpful message |

---

### v1.4.0 — Infrastructure & Performance

| Feature | Priority | Notes |
|---------|----------|-------|
| GPU inference support | Medium | Optional GPU acceleration for sub-second classification — useful for high-traffic deployments |
| Model caching strategy | Medium | Pre-warm the model on startup to reduce latency for the first request after a cold start |
| Webhook mode option | Low | Alternative to polling for platforms with inbound HTTPS support |
| Rate limiting | Low | Per-user request throttling to prevent abuse |

---

## Ideas Under Consideration

These are exploratory ideas — not committed, not prioritised, just interesting:

| Idea | Description |
|------|-------------|
| **ABCDE Analysis Mode** | A guided flow where the bot asks the user to send multiple photos and describe the lesion's asymmetry, border, colour, diameter, and evolution — then provides a holistic assessment |
| **Dermatologist Finder** | After a high-severity result, provide location-based links to find local dermatologists |
| **Educational Mode** | Explain each of the 7 HAM10000 conditions with example images and descriptions, accessible via a `/learn` command |
| **Second-opinion Mode** | Allow the user to tag a result as "I disagree" and trigger an alternative analysis or different Gemini prompt |
| **Anonymised Research Dataset** | Opt-in contribution of anonymised classification results (no images) to help track model accuracy in the wild |

---

## What Won't Be Added

Some things that might seem obvious but won't happen in this project:

| Feature | Why Not |
|---------|---------|
| Persistent image storage | Privacy. Full stop. See [Privacy](Privacy.md). |
| Automated medical diagnosis / treatment recommendations | This is an educational tool. Hard-coded guardrails are intentional. |
| Subscription / payment integration | Scope creep. Out of scope for a research bot. |
| A web interface | Use the Telegram app. That's the platform. |

---

## Contributing to the Roadmap

Have an idea? Think something on this list should be prioritised differently?

Open an issue with the `enhancement` label. Describe:
- The feature
- Why it matters for users
- Any implementation considerations you've thought through

Good ideas get implemented. Great ideas get implemented fast.
