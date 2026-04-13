# Memory System Breakdown — Rae's Architecture

> Documented 2026-04-06 for operational reference

## Overview

Three-part memory architecture for the permanent Hermes install. The original plan documented here assumed Chroma + Redis, but the current Hermes implementation works differently. The live system has three distinct memory/retrieval mechanisms:

1. Built-in curated memory (`MEMORY.md` / `USER.md`)
2. Session recall (`state.db` + `session_search`)
3. One optional external memory provider (`memory.provider`)

---

## Layer 1: Core Memory (Built-in)

**Technology:** File-based (`MEMORY.md`, `USER.md`)  
**Location:** `~/.hermes/memories/`  
**Status:** ✅ Active by default

### How It Works
- Two files: `MEMORY.md` (general facts) and `USER.md` (user preferences/profile)
- Injected into system prompt at session start as frozen snapshot
- Leverages LLM prefix caching for efficiency
- Agent actively manages via tools: `memory_add`, `memory_replace`, `memory_remove`, `memory_view`

### Configuration
```yaml
memory:
  memory_enabled: true
  user_profile_enabled: true
  memory_char_limit: 2200   # ~800 tokens
  user_char_limit: 1375     # ~500 tokens
```

### Management Rules
- 80% threshold: when memory usage exceeds 80%, agent should consolidate entries
- Best practice: merge multiple specific entries into comprehensive summaries
- Character limits ensure system prompt integrity

---

## Layer 2: Session Recall (Transcript Search)

**Technology:** SQLite + FTS5 (`SessionDB`)  
**Location:** `~/.hermes/state.db`  
**Status:** ✅ Active by default

### How It Works
- Every conversation is persisted into the shared session database
- `session_search` queries past sessions and returns summaries of what happened
- Good for “what did we do last time?” and transcript-style recall
- This is separate from `MEMORY.md` / `USER.md` and separate from external memory providers

### When to Use
- Recovering prior work from earlier sessions
- Looking up previous decisions, experiments, or outcomes
- Cross-session recall when the fact was discussed but never promoted into curated memory

---

## Layer 3: External Memory Provider (Optional)

**Technology:** One provider selected via `memory.provider`  
**Location:** Provider-specific  
**Status:** Optional — built-in memory still stays active

### How It Works
- Hermes always keeps built-in file memory enabled
- You may enable exactly one external provider at a time
- Providers plug into the `MemoryProvider` interface and can add tools, prefetch, and post-turn/session extraction
- Examples available in the current codebase: `holographic`, `honcho`, `openviking`, `mem0`, `hindsight`, `retaindb`, `byterover`, `supermemory`

### Current Reality
- `holographic` is local SQLite + FTS5, not Redis and not Chroma
- No Redis-backed working-memory layer exists in the live Hermes stack
- No built-in Chroma vector layer is wired into the default memory path

### When to Use
- You need richer recall than curated file memory alone
- You want provider-specific capabilities such as semantic search, dialectic user modeling, or knowledge-graph style storage
- You are willing to configure and maintain one provider explicitly

---

## External Memory Providers (Advanced)

Hermes supports pluggable memory providers via `memory.provider` config:

| Provider | Type | Status |
|----------|------|--------|
| `mem0` | Cloud API + embeddings | Requires API key |
| `holographic` | Local SQLite + FTS5 | Available |
| `honcho` | AI-native memory | Available |
| `openviking` | Session-managed | Available |
| `hindsight` | Knowledge graph | Available |
| `retaindb` | Cloud hybrid | Requires API key |

**Config activation:**
```yaml
memory:
  provider: mem0  # pick ONE
```

**Note:** Only ONE external provider at a time. Built-in file memory always active.

---

## Architecture Diagram

```
┌─────────────────────────────────────────────┐
│           HERMES AGENT                       │
│         (Memory Plugin Layer)                │
├─────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐            │
│  │ MEMORY.md   │  │  USER.md    │  ← Core   │
│  │ (curated)   │  │ (profile)   │  (file)   │
│  └──────┬──────┘  └──────┬──────┘            │
│         │                │                    │
│         └────────────────┼────────────────┐  │
│                          ▼                   │
│              ┌───────────────────────┐        │
│              │   Memory / Recall    │        │
│              │      Routing         │        │
│              └───────────┬───────────┘        │
└──────────────────────────┼───────────────────┘
                           │
      ┌────────────────────┼────────────────────┐
      ▼                    ▼                    ▼
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│  SESSIONDB  │      │ BUILT-IN    │      │ EXTERNAL    │
│ SQLite/FTS5 │      │ FILE MEMORY │      │ PROVIDER    │
│             │      │             │      │ (ONE ACTIVE)│
│ session_    │      │ MEMORY.md   │      │ holographic │
│ search      │      │ USER.md     │      │ honcho etc. │
└─────────────┘      └─────────────┘      └─────────────┘
```

---

## Schema Contract

All memory backends implement this interface:

```python
memory_store(entry_type: str, content: str, metadata: dict = {})
memory_retrieve(query: str, entry_type: str = None, limit: int = 5)
```

As long as plugins implement this, backends can be swapped without touching the agent.

---

## Rae's Memory Toolkit

| Tool | Layer | Purpose |
|------|-------|---------|
| `memory_add` | Core (File) | Add entry to MEMORY.md |
| `memory_replace` | Core (File) | Update existing entry |
| `memory_remove` | Core (File) | Delete entry |
| `memory_view` | Core (File) | Show current memory |
| `session_search` | Session Recall | Search/summarize past conversations from `state.db` |
| Provider tools | External | Provider-specific tools exposed by the active memory plugin |

---

## Ollama Integration — rae-gemma4

Rae has a custom Ollama model with her personality baked in:

**Model:** `rae-gemma4`  
**Created:** 2026-04-07  
**Context:** 32k tokens  
**Base:** gemma4:e2b  
**System prompt:** SOUL.md injected via Modelfile

**Config:**
```yaml
model:
  default: rae-gemma4
  provider: custom
  base_url: http://localhost:11434/v1
fallback_providers:
- provider: custom
  model: rae-gemma4
  context_length: 32768
```

**Usage:** This is the first fallback in the cascade — when main model fails, Rae responds with her own voice (SOUL.md baked in).

---

## Key Points for Rae

1. **File memory is always on** — no config needed, used by default
2. **Session search is separate** — use it to recover prior transcripts/work
3. **External memory is optional** — one provider at a time via `memory.provider`
4. **Redis is not part of the live stack** — old note, now corrected
5. **Don't overthink** — start with file memory, add providers only when needed
6. **80% rule** — consolidate curated memory when it gets full

---

## Future Possibilities

- Neo4j for graph-based memory (entity relationships)
- Mem0 cloud for advanced embeddings
- Custom plugin for specific use cases

The architecture is modular — new backends plug in via the same interface.