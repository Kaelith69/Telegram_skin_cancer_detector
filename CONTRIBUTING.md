# 🤝 Contributing to Skin Cancer Detector Bot

First off: thank you for even reading this file. Most contributors skip straight to opening a PR and then wonder why it got closed. You are already ahead of the curve.

---

## 🐛 Found a Bug?

Congratulations. You are now officially part of the development team. Your equity vests in 4 years with a 1-year cliff.

Please [open an issue](https://github.com/Kaelith69/Telegram_skin_cancer_detector/issues) and include:

- A clear, descriptive title (not "it doesn't work")
- Steps to reproduce (numbered, please — we're not archaeologists)
- What you expected to happen
- What actually happened (bonus points for logs or screenshots)
- Your Python version and OS

---

## 💡 Have a Feature Idea?

Open an issue first. Seriously. Before you write a single line of code, let's talk about it. That way we don't both spend a weekend building the same thing from opposite directions.

Use the **Feature Request** label and describe:

- The problem you're solving
- Your proposed solution
- Why this belongs in the bot rather than in your fork

---

## 🛠️ Making Code Changes

### 1. Fork and clone

```bash
git clone https://github.com/YOUR_USERNAME/Telegram_skin_cancer_detector.git
cd Telegram_skin_cancer_detector
```

### 2. Create a branch

```bash
git checkout -b feature/my-brilliant-idea
# or
git checkout -b fix/that-embarrassing-bug
```

Branch naming conventions:
- `feature/` — new functionality
- `fix/` — bug fixes
- `docs/` — documentation only
- `refactor/` — code changes that don't add features or fix bugs

### 3. Set up your environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# Fill in your real tokens in .env
```

### 4. Make your changes

Keep `bot.py` clean. It's one file. Please don't turn it into a monorepo.

Code style expectations:
- Use descriptive variable names. `x` is not a variable name, it's a cry for help.
- Add a comment if the logic is non-obvious. Future you (and us) will thank present you.
- Keep functions focused. If a function needs a 10-line docstring to explain what it does, consider splitting it.
- Don't commit your `.env` file. Seriously. We will notice.

### 5. Test manually

```bash
python bot.py
```

Send a test image through your Telegram bot instance. Verify:
- The image gets classified
- Gemini returns an explanation
- The temp file is deleted after processing
- Error cases (no photo, API down) return friendly messages

### 6. Open a Pull Request

```bash
git push origin feature/my-brilliant-idea
```

In your PR description, include:
- **What**: what does this change?
- **Why**: why is this change needed?
- **How**: any implementation notes worth highlighting?
- **Testing**: what did you test, and how?

---

## 📐 Code Style

We're not religious about formatting, but we are opinionated about a few things:

- **Indentation:** 4 spaces. Tabs will be rejected. This is not a negotiation.
- **Line length:** Keep it under 110 characters where possible.
- **String quotes:** Single quotes for internal strings, double for format strings and docstrings. Or just be consistent within a file.
- **Type hints:** Nice to have, especially for new functions. The existing `tuple[str, str]` return type is a good example.

---

## 🚫 What We Won't Merge

- Code that stores user images beyond the temp file lifecycle
- Hardcoded API keys or tokens (instant rejection)
- Changes that break the ephemeral image processing guarantee
- Features that add new required environment variables without updating `.env.example`
- Removing the medical disclaimer

---

## 🙏 Thank You

Open source is about humans helping humans. Whether you're fixing a typo or adding GPU inference support — it matters. Thank you for making this better.

Now go wash your hands. You've been touching code.
