# Bounded Memory

[![Version](https://img.shields.io/badge/version-v1.0.4-blue.svg)](https://github.com/canmaxice-maker/bounded-memory)
[![ClawHub](https://img.shields.io/badge/ClawHub-bounded--memory-green.svg)](https://clawhub.com/bounded-memory)

**Gives your OpenClaw AI a perfect memory.** Search through all your past conversations instantly — recall decisions, preferences, and context from months ago.

## The Problem It Solves

OpenClaw AI starts fresh every session. It forgets everything from previous chats.

That means every new session, you have to re-explain:
- Your projects and preferences
- Past decisions you've made
- What you discussed and agreed on
- Context from earlier conversations

**Bounded Memory fixes this.** It gives your AI an always-on memory of everything you've ever discussed.

## What You Can Do

```
"Did we discuss the logo design?"
→ Instantly finds all relevant past conversations

"What did we decide about the budget?"
→ Shows you the exact discussion and decision

"Find that conversation about the robot project from last month"
→ Pulls it up immediately
```

## How It Works

```
1. Index (once)
   Your conversation files → Local search database (SQLite)

2. Search (anytime you ask)
   Your question → Instant results from full history

3. Recall
   You get the answer + the original conversation context
```

Everything runs **offline on your machine** — no cloud, no external services.

## Installation

```bash
# ClawHub (recommended)
clawhub install bounded-memory

# Manual
git clone https://github.com/canmaxice-maker/bounded-memory.git
mv bounded-memory ~/.openclaw/workspace/main/skills/session-search
```

## Quick Start

```bash
# 1. Index your conversations (first time)
python3 ~/.openclaw/workspace/main/skills/session-search/scripts/index-sessions.py --agent main

# 2. Search
python3 ~/.openclaw/workspace/main/skills/session-search/scripts/search-sessions.py "your question"

# 3. Optional: enable AI summaries of results
python3 ~/.openclaw/workspace/main/skills/session-search/scripts/search-sessions.py "question" --limit 5
```

Set up daily auto-indexing so your AI always has fresh memory:
```bash
# Runs automatically every day at 8pm
# (configured via cron after install)
```

## What Changes

| Before | After |
|--------|-------|
| "I know we discussed this before but..." | "Found it — we talked about this on April 3rd and decided to..." |
| AI has no idea what you asked last month | AI instantly recalls months of conversations |
| Re-explaining context every session | Context carries across all sessions automatically |
| Forgetting important decisions | Never lose track of what was decided |

## Privacy

- **100% local** — all data stays on your machine
- **No cloud** — no external services involved in search
- **You control it** — uninstall anytime, remove the database anytime
- **Git-ignored** — your conversation database is never committed to version control

## Architecture

```
~/.openclaw/agents/main/sessions/*.jsonl
    ↓ (indexed once, updated daily)
~/.openclaw/workspace/main/skills/session-search/db/sessions.db
    ↓ (searched on demand)
Instant results from your full conversation history
```

## Credits

Inspired by [Hermes Agent](https://github.com/NousResearch/hermes-agent)'s bounded memory architecture.

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| [v1.0.4](https://github.com/canmaxice-maker/bounded-memory/releases/tag/v1.0.4) | 2026-04-21 | Convert docs to English, professional quality |
| [v1.0.3](https://github.com/canmaxice-maker/bounded-memory/releases/tag/v1.0.3) | 2026-04-21 | Security transparency, LLM opt-in/out |
| [v1.0.0](https://github.com/canmaxice-maker/bounded-memory/releases/tag/v1.0.0) | 2026-04-21 | Initial release |

## License

MIT
