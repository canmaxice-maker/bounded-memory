---
name: bounded-memory
description: Gives your OpenClaw AI a perfect memory. Ask things like "did we discuss this before?", "what did we decide about X?", and "find that conversation about Y" — it searches through all your past conversations instantly. Great for recalling decisions, preferences, and context from months ago. Use when: (1) user asks "did we talk about X before?", (2) "search my old conversations", (3) "find what we decided about project Y", (4) "remember what I asked last week". Triggers: "search sessions", "find earlier conversation", "recall past discussion", "what did I say about".

NOTE: All conversation data stays on your machine — nothing is sent externally during indexing. LLM summarization is optional and disabled by default (use --no-llm to disable, or --llm to enable). Works fully offline.
---

# Bounded Memory

Gives your OpenClaw AI agent a **perfect memory** — it can recall anything you've ever discussed, even from months ago.

## What It Does

Without this skill: each OpenClaw session starts fresh. The AI forgets everything from previous chats.

With this skill: you can ask things like:
- "Did we discuss X before?"
- "What did we decide about Y?"
- "Find that conversation about Z from last month"

And get instant answers from your full conversation history.

## How It Works

1. **Index** — Automatically scans all your past conversation files (runs once, then incrementally updates)
2. **Search** — When you ask about something, it instantly finds all relevant past conversations
3. **Recall** — You get the answer + context from the original discussion

No cloud services. Everything stays on your device.

## Quick Start

```bash
# Index your conversations (first time)
python3 skills/session-search/scripts/index-sessions.py --agent main

# Ask about something
python3 skills/session-search/scripts/search-sessions.py "what did we decide about the logo design"

# Ask with optional AI summary
python3 skills/session-search/scripts/search-sessions.py "your question" --limit 5
```

## What It Solves

| Problem | Without | With Bounded Memory |
|---------|---------|---------------------|
| "I asked this before but can't remember the answer" | AI has no idea | Instant recall from history |
| "What did we decide in that meeting?" | Forgot | Searches all past sessions |
| "Did I mention this to the AI before?" | No way to know | Searches everything |

## Example

```
You: "Search our conversations about the N-Fellow robot project"
→ Found 3 discussions:
  1. [Last week] We discussed the design direction...
  2. [2 weeks ago] You asked about pricing for...
  3. [Last month] The AI suggested adding...
```

## Privacy

- All data stored locally (SQLite on your machine)
- No external services for search
- Optional AI summary — disabled by default, opt-in
- Nothing leaves your device
