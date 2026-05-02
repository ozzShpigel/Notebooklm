# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

This is a workspace for generating Hebrew audio podcasts from books using the NotebookLM CLI (`notebooklm-py`). It contains downloaded MP3 files and is not a traditional software project.

## NotebookLM CLI Usage

All commands require `PYTHONIOENCODING=utf-8` prefix on Windows to avoid cp1252 encoding crashes with Hebrew output. Always use `--json` for machine-parseable output.

```bash
PYTHONIOENCODING=utf-8 python -m notebooklm list --json
PYTHONIOENCODING=utf-8 python -m notebooklm generate audio "instructions" --language he --json
PYTHONIOENCODING=utf-8 python -m notebooklm download audio ./output.mp3 -a <artifact_id> -n <notebook_id>
```

Authentication expires frequently. Re-authenticate with:
```bash
python -m notebooklm login
```

## Workflow

1. Create or select a notebook (`notebooklm create` / `notebooklm use`)
2. Add book sources (`notebooklm source add <file>`)
3. Ask for top chapters (`notebooklm ask "..." --json`)
4. Generate Hebrew podcast (`notebooklm generate audio "..." --language he --json`)
5. Wait via background agent, then download MP3 to this directory

## Key Constraints

- Audio generation takes 10-20 minutes; use background agents with `artifact wait --timeout 1200`
- Google rate-limits podcast generation; wait 5-10 min between retries if generation fails
- Use explicit `--notebook <id>` and `-a <artifact_id>` flags instead of relying on context when running parallel agents
