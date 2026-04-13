# Janet Memory System — 2-Part Architecture

> Implemented 2026-04-13. Validated 13/13 tests passed.

## Overview

Janet runs on the MacBook Pro 2016 as the guardian/sentinel agent. Her memory system is a 2-part architecture: built-in file memory as the always-on foundation, plus mem0 semantic memory for server-side extraction and retrieval.

**Principle:** Janet does not need the full 3-part redundancy of Rae. Her role is monitoring, healing, and protecting — memory recall is tool-assisted, not the primary function. A 2-part system is sufficient and keeps the stack lean.

---

## Architecture

```
┌──────────────────────────────────────────────┐
│          HERMES AGENT (Janet)                │
├──────────────────────────────────────────────┤
│  Layer 1: Built-in File Memory (always-on)   │
│    ~/.hermes/memories/MEMORY.md              │
│    ~/.hermes/memories/USER.md                │
│    → memory tool (add/replace/remove/view)   │
├──────────────────────────────────────────────┤
│  Layer 2: mem0 Semantic Memory               │
│    Provider: mem0 (cloud API)                │
│    ~/.hermes/mem0.json                       │
│    → mem0_search, mem0_conclude,             │
│      mem0_profile                            │
├──────────────────────────────────────────────┤
│  Layer 3: Session History (always-on)         │
│    ~/.hermes/state.db (FTS5)                 │
│    → session_search tool                     │
└──────────────────────────────────────────────┘
```

**Configured provider:** `mem0`
**Memory provider slot:** `memory.provider = 'mem0'` in config.yaml
**mem0 API key:** stored in `~/.hermes/.env` (MEM0_API_KEY)

---

## Layer 1 — Built-in File Memory

**Technology:** Plain text files (Markdown)
**Location:** `~/.hermes/memories/MEMORY.md` and `USER.md`
**Status:** Always active, cannot be disabled

### How It Works
- `MEMORY.md` — operational facts Janet needs to function (network paths, service configs, Tony's preferences)
- `USER.md` — Tony's identity, communication style, and key preferences
- Both injected into system prompt at session start
- Managed via `memory` tool: `memory_add`, `memory_replace`, `memory_remove`

### Configuration
```yaml
memory:
  memory_enabled: true
  user_profile_enabled: true
  memory_char_limit: 1500
  user_char_limit: 1375
  consolidation_threshold: 0.8
```

### Tools
| Tool | Action | Use |
|------|--------|-----|
| `memory` | add | Add entry to MEMORY.md |
| `memory` | replace | Update existing entry |
| `memory` | remove | Delete entry |
| `memory` | view | Show current memory |
| `memory` | search | Search memory entries |

---

## Layer 2 — mem0 Semantic Memory

**Technology:** mem0 Platform (cloud API with embeddings)
**Location:** `~/.hermes/mem0.json` (config), mem0 cloud (storage)
**Status:** Active — `memory.provider = 'mem0'`
**Package:** `mem0ai` v1.0.11 installed

### How It Works
- Server-side LLM extraction from conversation turns
- Semantic search with reranking
- Automatic deduplication
- Scoped per user_id (janet) and agent_id (hermes)

### Configuration
```json
// ~/.hermes/mem0.json
{
  "api_key": "<from MEM0_API_KEY>",
  "user_id": "janet",
  "agent_id": "hermes",
  "rerank": true
}
```

### Tools
| Tool | Purpose |
|------|---------|
| `mem0_search` | Semantic search across all stored memories |
| `mem0_conclude` | Store a durable explicit fact verbatim |
| `mem0_profile` | Retrieve all memories about the user |

### Key Design Notes
- **API key required:** Free tier at https://app.mem0.ai
- **Circuit breaker:** After 5 consecutive failures, pauses for 120s to avoid hammering a down server
- **Non-blocking sync:** `sync_turn()` fires in background thread — does not block response latency
- **Prefetch:** `queue_prefetch()` runs async background prefetch on conversation start

---

## Layer 3 — Session History (Always-On)

**Technology:** SQLite + FTS5
**Location:** `~/.hermes/state.db`
**Status:** Always active — Hermes core feature, not configurable

### How It Works
- Full conversation transcript stored per session
- Full-text search index over all messages
- `session_search` tool for cross-session recall
- Separate from the memory plugin system — always available regardless of provider

---

## Installation Log

```
STEP 1: pip install mem0ai           → mem0ai 1.0.11 installed
STEP 2: Configure ~/.hermes/mem0.json → api_key, user_id=janet
STEP 3: Set memory.provider='mem0'   → config.yaml updated
STEP 4: hermes restart              → provider active
```

---

## Validation

Test suite: `~/.hermes/scripts/test_janet_memory.py`
Run: `HERMES_HOME=~/.hermes python test_janet_memory.py`

Results: **13/13 PASSED** (2026-04-13)

Tests cover:
1. mem0ai package installed
2. MEM0_API_KEY present in environment
3. config.yaml memory.provider = 'mem0'
4. Mem0MemoryProvider.is_available() = True
5. mem0 tool schemas registered (mem0_search, mem0_conclude, mem0_profile)
6. mem0_search functional (API reachable)
7. mem0_conclude stores and retrieves facts
8. MEMORY.md and USER.md exist
9. 'memory' tool registered in registry
10. Session history accessible (103+ sessions)
11. Janet provider is NOT holographic (Rae isolation)
12. Exactly one external provider configured
13. 2-part architecture manifest confirmed

---

## Janet vs Rae Memory Isolation

| Property | Janet (MBP) | Rae (M4) |
|----------|-------------|-----------|
| Architecture | 2-part | 3-part |
| External provider | mem0 | holographic |
| Provider config | `memory.provider: 'mem0'` | `memory.provider: 'holographic'` |
| Redis | Not used | Not used |
| Chroma | Not used | Not used |
| file memory | MEMORY.md + USER.md | MEMORY.md + USER.md |
| Session history | state.db (always-on) | state.db (always-on) |

---

## Troubleshooting

**mem0 returns "API temporarily unavailable":**
→ Circuit breaker tripped after 5 consecutive failures. Waits 120s then retries automatically.

**mem0 not available (is_available = False):**
→ Check MEM0_API_KEY is set in `~/.hermes/.env` and `mem0ai` package is installed.

**memory tool not found:**
→ Ensure tools are loaded: `/reset` in CLI or restart gateway.

**Session search returns no results:**
→ session_search is always-on — check `~/.hermes/state.db` permissions.
