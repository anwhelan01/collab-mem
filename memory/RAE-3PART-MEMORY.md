# Rae Memory System — 3-Part Redundancy Architecture

> Implemented 2026-04-13. Validated 11/11 tests passed.

## Overview

Rae runs on the M4 Mac Mini as the chief-of-staff agent. Her memory system is a 3-part redundancy architecture: built-in file memory as the always-on foundation, holographic structured memory for entity-aware fact storage with trust scoring, and session history for cross-session recall.

**Principle:** Rae is the primary operational agent — memory recall must be deep, structured, and redundant. The 3-part system ensures no single failure can cause memory loss and provides multiple retrieval mechanisms suited to different query types.

---

## Architecture

```
┌──────────────────────────────────────────────────────────┐
│          HERMES AGENT (Rae)                               │
├──────────────────────────────────────────────────────────┤
│  Layer 1: Built-in File Memory (always-on)              │
│    ~/.hermes/memories/MEMORY.md                          │
│    ~/.hermes/memories/USER.md                            │
│    → memory tool (add/replace/remove/view)               │
├──────────────────────────────────────────────────────────┤
│  Layer 2: Holographic Structured Memory                 │
│    ~/.hermes/memory_store.db (SQLite)                    │
│    → fact_store, fact_feedback                           │
│    (entity resolution, trust scoring, HRR retrieval)     │
├──────────────────────────────────────────────────────────┤
│  Layer 3: Session History (always-on)                    │
│    ~/.hermes/state.db (FTS5)                             │
│    → session_search tool                                 │
└──────────────────────────────────────────────────────────┘
```

**Configured provider:** `holographic`
**Memory provider slot:** `memory.provider = 'holographic'` in config.yaml
**Holographic DB:** `~/.hermes/memory_store.db`

---

## Layer 1 — Built-in File Memory

**Technology:** Plain text files (Markdown)
**Location:** `~/.hermes/memories/MEMORY.md` and `USER.md`
**Status:** Always active, cannot be disabled

### How It Works
- `MEMORY.md` — Rae's operational facts (Discord config, Google Workspace accounts, SSH paths, Tony's preferences)
- `USER.md` — Tony's identity, communication style, key preferences
- Both injected into system prompt at session start
- Managed via `memory` tool

### Configuration
```yaml
memory:
  memory_enabled: true
  user_profile_enabled: true
  memory_char_limit: 2200
  user_char_limit: 1375
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

## Layer 2 — Holographic Structured Memory

**Technology:** Holographic (local SQLite + FTS5 + HRR)
**Location:** `~/.hermes/memory_store.db`
**Status:** Active — `memory.provider = 'holographic'`
**Source:** `plugins/memory/holographic/` in hermes-agent

### How It Works

Holographic memory stores structured facts with:

1. **Entity resolution** — facts are tagged to entities (person, project, tool, general)
2. **Trust scoring** — each fact has a trust score (0.0–1.0) that evolves via feedback
3. **HRR (Holographic Reduced Representation)** — compositional vector retrieval
4. **Categories:** `user_pref`, `project`, `tool`, `general`
5. **Actions:** add, search, probe, related, reason, contradict, update, remove, list

### Configuration
```yaml
# ~/.hermes/config.yaml
plugins:
  hermes-memory-store:
    db_path: ~/.hermes/memory_store.db
    auto_extract: false          # true = auto-extract facts from conversation on session end
    default_trust: 0.5
    min_trust_threshold: 0.3
    hrr_dim: 1024
    hrr_weight: 0.3
    temporal_decay_half_life: 0  # 0 = no decay
```

### Tools
| Tool | Action | Purpose |
|------|--------|---------|
| `fact_store` | add | Store structured fact with category and tags |
| `fact_store` | search | Keyword lookup in fact store |
| `fact_store` | probe | Entity recall — all facts about a person/thing |
| `fact_store` | related | Structural adjacency — what connects to an entity |
| `fact_store` | reason | Compositional — facts connected to MULTIPLE entities |
| `fact_store` | contradict | Find conflicting facts (memory hygiene) |
| `fact_store` | list | List all facts with optional category/trust filter |
| `fact_store` | update/remove | CRUD operations |
| `fact_feedback` | helpful/unhelpful | Rate fact accuracy — trains trust scores |

### Design Notes
- **Auto-extraction off by default** — facts are stored explicitly via tools, not auto-scraped
- **Trust scoring** — new facts start at 0.5, adjust via `fact_feedback` after use
- **on_memory_write hook** — holographic mirrors `memory_add` writes from built-in file memory
- **No API key required** — fully local, self-hosted
- **SQLite backend** — persistent, fast, zero external dependencies

### Example Usage

```python
# Store a fact
fact_store(action='add', content='Tony prefers brief technical reports', category='user_pref', tags='reporting')

# Probe all facts about Tony
fact_store(action='probe', entity='Tony')

# Reason across multiple entities
fact_store(action='reason', entities=['Tony', 'Discord'], limit=5)

# Rate accuracy after using a fact
fact_feedback(action='helpful', fact_id=42)
```

---

## Layer 3 — Session History (Always-On)

**Technology:** SQLite + FTS5
**Location:** `~/.hermes/state.db`
**Status:** Always active — Hermes core feature, not configurable

### How It Works
- Full conversation transcript stored per session
- Full-text search index over all messages
- `session_search` tool for cross-session recall
- 110+ sessions retained (as of 2026-04-13)

---

## Installation Log

```
STEP 1: Verify holographic plugin exists    → plugins/memory/holographic/ present
STEP 2: Check numpy dependency             → numpy available in hermes venv
STEP 3: Set memory.provider='holographic'  → config.yaml updated
STEP 4: Configure hermes-memory-store       → plugins.hermes-memory-store in config.yaml
STEP 5: hermes restart                     → holographic provider active
```

---

## Validation

Test suite: `~/.hermes/scripts/test_rae_memory.py`
Run: `python test_rae_memory.py` (via SSH to M4)

Results: **11/11 PASSED** (2026-04-13)

Tests cover:
1. Holographic plugin source present
2. config.yaml memory.provider = 'holographic'
3. HolographicMemoryProvider.is_available() = True
4. fact_store and fact_feedback schemas registered
5. fact_store add/list/remove CRUD functional
6. fact_store probe entity recall functional
7. MEMORY.md and USER.md exist on M4
8. Session history accessible (110 sessions)
9. 3-part architecture manifest confirmed
10. Rae provider is NOT mem0 (Janet isolation)
11. on_memory_write hook mirrors built-in writes

---

## Janet vs Rae Memory Isolation

| Property | Janet (MBP) | Rae (M4) |
|----------|-------------|-----------|
| Architecture | 2-part | 3-part |
| External provider | mem0 | holographic |
| Provider config | `memory.provider: 'mem0'` | `memory.provider: 'holographic'` |
| API key required | Yes (mem0.ai) | No (fully local) |
| Redis | Not used | Not used |
| Chroma | Not used | Not used |
| file memory | MEMORY.md + USER.md | MEMORY.md + USER.md |
| Session history | state.db (always-on) | state.db (always-on) |
| Unique capability | Semantic search (LLM extraction) | Entity reasoning + trust scoring |

---

## Troubleshooting

**fact_store returns error:**
→ Check `~/.hermes/memory_store.db` file permissions. SQLite needs write access.

**Holographic not available (is_available = False):**
→ numpy must be installed: `pip install numpy`. Already present in hermes-agent venv.

**auto_extract not working:**
→ Set `auto_extract: true` in `plugins.hermes-memory-store` config section.

**Trust scores all 0.5:**
→ Normal for new facts. Use `fact_feedback(action='helpful')` after using a fact to increase its trust.

**Conflicting facts:**
→ Use `fact_store(action='contradict')` to find facts making conflicting claims.
