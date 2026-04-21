---
name: session-search
description: FTS5 full-text search over OpenClaw session histories. Indexes session .jsonl files into SQLite with FTS5, then searches with optional LLM summarization. Use when: (1) user asks to search/recall past conversations, (2) "did we discuss X before?", (3) finding previous decisions or context from old sessions. Triggers on phrases like "search sessions", "did we talk about", "find earlier conversation", "look up what we discussed".
---

# Session Search

SQLite FTS5-powered session history search for OpenClaw. Indexes all session `.jsonl` files and provides fast full-text search with optional MiniMax LLM summarization.

## Setup

```bash
# Index sessions (first time — full index)
python3 skills/session-search/scripts/index-sessions.py

# Search
python3 skills/session-search/scripts/search-sessions.py "关键词"

# Options
python3 skills/session-search/scripts/search-sessions.py "query" --limit 10 --no-llm
```

## Configuration

The skill auto-detects the OpenClaw agents directory (`~/.openclaw/agents`) and writes the SQLite DB to `skills/session-search/db/sessions.db` (relative to the skill directory).

Override with env vars:
- `OPENCLAW_AGENTS_DIR` — session files location
- `SESSION_SEARCH_DB` — SQLite database path

## Workflow

1. **Index** — Parse all `.jsonl` session files, extract user/assistant messages, store in FTS5
2. **Search** — FTS5 MATCH with BM25 ranking, optionally LLM summarize results

## Script Reference

| Script | Purpose |
|--------|---------|
| `index-sessions.py` | Scan + index session files. Use `--incremental` to skip unchanged files. |
| `search-sessions.py` | Search indexed sessions. `--limit N` sets result count. `--no-llm` skips summarization. |

## Indexing

```bash
# Full index (all sessions)
python3 skills/session-search/scripts/index-sessions.py --agent main

# Incremental (only changed files, fast)
python3 skills/session-search/scripts/index-sessions.py --agent main --incremental
```

Set up a daily cron for incremental updates:
```
0 20 * * *  python3 ~/.openclaw/workspace/main/skills/session-search/scripts/index-sessions.py --agent main --incremental
```

## Search Examples

```bash
# Basic keyword search
python3 skills/session-search/scripts/search-sessions.py "project setup"

# With LLM summary (requires API key)
python3 skills/session-search/scripts/search-sessions.py "database schema" --limit 5

# No LLM, more results
python3 skills/session-search/scripts/search-sessions.py "deployment config" --limit 10 --no-llm
```
