# NotebookLM Hebrew Podcasts

A collection of Hebrew audio podcasts generated from books using [Google NotebookLM](https://notebooklm.google.com/) and the [`notebooklm-py`](https://pypi.org/project/notebooklm-py/) CLI.

## Overview

This workspace automates the process of turning books into focused Hebrew podcasts. For each book, the top 3 most important chapters are identified and used to generate an in-depth audio discussion.

## Podcasts

| Book | Author | Topics |
|------|--------|--------|
| Atomic Habits | James Clear | Habit loops, identity-based habits, environment design |
| Code Complete | Steve McConnell | Software construction, design, quality |
| Meditations | Marcus Aurelius | Stoic philosophy, virtue, self-discipline |
| The Pragmatic Programmer | Hunt & Thomas | Career craft, pragmatic philosophy, code flexibility |
| Clean Code | Robert C. Martin | Naming, functions, formatting |
| Preying Mantis Kung Fu: Kicking Fundamentals | — | Yin/Yang theory, kicking power, combat applications |
| Shaolin Kung Fu | — | Shaolin martial arts techniques |
| The Way of Qigong | Kenneth S. Cohen | Qi fundamentals, posture, scientific research |

## Workflow

```
Select book → Identify top 3 chapters → Generate Hebrew podcast → Download MP3
```

1. **Create notebook** and add the book as a source
2. **Ask NotebookLM** to identify the 3 most important chapters
3. **Generate audio** with Hebrew language and chapter-focused instructions
4. **Download** the completed MP3 (generation takes 10–20 min)

## Prerequisites

- Python 3.10+
- Google account with [NotebookLM](https://notebooklm.google.com/) access

## Setup

All podcast generation is powered by [`notebooklm-py`](https://github.com/teng-lin/notebooklm-py) — an open-source Python CLI and API for Google NotebookLM. This is **not** a local script in this repo; it is a pip-installable package that provides the `notebooklm` command.

> [!TIP]
> Full CLI documentation, supported commands, and Python API reference are available at the [notebooklm-py GitHub repository](https://github.com/teng-lin/notebooklm-py).

### Install

```bash
pip install notebooklm-py

# Optional: browser login support (required for first-time auth)
pip install "notebooklm-py[browser]"
playwright install chromium
```

### Authenticate

```bash
notebooklm login          # Opens browser for Google OAuth
notebooklm list           # Verify authentication works
```

> [!NOTE]
> On Windows, prefix all commands with `PYTHONIOENCODING=utf-8` to avoid encoding issues with Hebrew output. Example: `PYTHONIOENCODING=utf-8 python -m notebooklm list --json`

## Usage

### 1. Create a notebook and add a book

```bash
python -m notebooklm create "Book Title" --json
python -m notebooklm source add ./book.epub --json
```

Supported source types: PDF, EPUB, URLs, YouTube links, Google Docs, audio, video, and images.

### 2. Identify the top chapters

```bash
python -m notebooklm ask "What are the 3 most important chapters?" --json
```

### 3. Generate a Hebrew podcast

```bash
python -m notebooklm generate audio \
  "Focus on chapters X, Y, and Z..." \
  --language he --json
```

Generation takes **10–20 minutes**. Check progress with:

```bash
python -m notebooklm artifact list --json
```

### 4. Download the MP3

```bash
python -m notebooklm download audio ./podcast.mp3
```

> [!WARNING]
> Google rate-limits podcast generation. If generation fails, wait 5–10 minutes before retrying.
