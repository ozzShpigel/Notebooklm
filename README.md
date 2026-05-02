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
- [`notebooklm-py`](https://pypi.org/project/notebooklm-py/) (`pip install notebooklm-py`)
- Google account with NotebookLM access

```bash
pip install notebooklm-py
notebooklm login
```

## Usage

```bash
# List notebooks
PYTHONIOENCODING=utf-8 python -m notebooklm list --json

# Create notebook and add a book
PYTHONIOENCODING=utf-8 python -m notebooklm create "Book Title" --json
PYTHONIOENCODING=utf-8 python -m notebooklm source add ./book.epub --json

# Generate Hebrew podcast
PYTHONIOENCODING=utf-8 python -m notebooklm generate audio \
  "Focus on the 3 most important chapters..." \
  --language he --json

# Download when ready
PYTHONIOENCODING=utf-8 python -m notebooklm download audio ./podcast.mp3
```

> [!NOTE]
> On Windows, the `PYTHONIOENCODING=utf-8` prefix is required to avoid encoding issues with Hebrew output.

> [!WARNING]
> Google rate-limits podcast generation. If generation fails, wait 5–10 minutes before retrying.
