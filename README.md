# Bounded Memory

Hermes-style bounded memory system for OpenClaw — SQLite FTS5 powered session history search.

Inspired by [Hermes Agent](https://github.com/NousResearch/hermes-agent)'s memory architecture, this skill brings structured session search to OpenClaw without relying on external vector databases.

## What It Does

OpenClaw agents are stateless by default — each session starts fresh. **Bounded Memory** indexes your conversation history into SQLite with FTS5 full-text search, so you can recall anything that's been discussed:

```
$ python3 search-sessions.py "N-Fellow robot project"
🔍 3 results:

1. [2026-04-18] 🤖 assistant
   已更新 ✅ 3DOO Maker 每日市场报告...
   
2. [2026-04-17] 👤 user
   这个会话id不是群，是单个飞书机器人...

3. [2026-04-15] 🤖 assistant
   👌🦞 今天辛...
```

## Features

| Feature | Description |
|---------|-------------|
| **FTS5 Full-Text Search** | BM25 ranking, handles hyphenated terms (e.g. `memory-core`) |
| **SQLite Only** | No external services — works offline, ~19MB per 16K messages |
| **Incremental Indexing** | Only re-indexes changed session files |
| **LLM Summarization** | Optional MiniMax/OpenAI summarization of search results |
| **Multi-Agent Support** | Index sessions across all agents or a specific one |
| **Privacy-First** | All data stays local — session DB excluded from git |

## Installation

### Option 1: ClawHub (recommended)

```bash
clawhub install bounded-memory
```

### Option 2: Manual

```bash
git clone https://github.com/canmaxice-maker/bounded-memory.git
mv bounded-memory ~/.openclaw/workspace/main/skills/session-search
```

## Quick Start

```bash
# 1. Index all sessions (first time)
python3 ~/.openclaw/workspace/main/skills/session-search/scripts/index-sessions.py --agent main

# 2. Search
python3 ~/.openclaw/workspace/main/skills/session-search/scripts/search-sessions.py "your query"

# 3. Search with LLM summary
python3 ~/.openclaw/workspace/main/skills/session-search/scripts/search-sessions.py "your query"
```

## Scripts

### `index-sessions.py`

Scan and index session `.jsonl` files into SQLite FTS5.

```bash
# Full index
python3 scripts/index-sessions.py --agent main

# Incremental (fast, skip unchanged files)
python3 scripts/index-sessions.py --agent main --incremental

# All agents
python3 scripts/index-sessions.py --all-agents
```

### `search-sessions.py`

FTS5 search with optional LLM summarization.

```bash
# Basic search
python3 scripts/search-sessions.py "止损 投资" --limit 5

# No LLM summary
python3 scripts/search-sessions.py "query" --no-llm

# More results
python3 scripts/search-sessions.py "query" --limit 10
```

## Architecture

```
~/.openclaw/agents/main/sessions/
    └── *.jsonl  (session transcript files)
          │
          ▼
    index-sessions.py
          │
          ▼
~/.openclaw/workspace/main/skills/session-search/
    └── db/sessions.db  (SQLite FTS5)
              │
              ▼
    search-sessions.py
              │
              ▼
    Results + optional LLM summary
```

**Schema:**
- `sessions_fts` — FTS5 virtual table with BM25 ranking
- `index_meta` — tracks index progress per session file

## Configuration

Override paths via environment variables:

| Variable | Default | Description |
|----------|---------|-------------|
| `OPENCLAW_AGENTS_DIR` | `~/.openclaw/agents` | Session files location |
| `SESSION_SEARCH_DB_DIR` | `<skill-dir>/db` | SQLite database directory |

## Credits

Bounded Memory is inspired by [Hermes Agent](https://github.com/NousResearch/hermes-agent)'s session search architecture. The concept of bounded-memory with strict character limits and frozen prompt snapshots originates from Hermes's memory system design.

## License

MIT
