# Memory System Breakdown — Rae's Architecture

> Documented 2026-04-06 for operational reference

## Overview

Three-layer memory architecture designed for the permanent Hermes install. Each layer serves a different purpose and can be swapped independently as the tech evolves.

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

## Layer 2: Semantic Memory (Vector)

**Technology:** Chroma (local vector database)  
**Location:** `~/.hermes/memory/chroma/`  
**Status:** ✅ Installed, skill available

### How It Works
- Embeddings-based similarity search
- Persistent storage to disk
- Self-hosted, no external service
- Use case: RAG recall, document retrieval, semantic search over facts

### Installation
- Skill: `optional-skills/mlops/chroma/SKILL.md` in Hermes
- Python deps: `chromadb`, `sentence-transformers` (already in venv)

### Usage Pattern
```python
import chromadb
client = chromadb.PersistentClient(path="~/.hermes/memory/chroma")
collection = client.create_collection("hermes_semantic")
# Add, query, retrieve semantic memories
```

### When to Use
- Long-term fact recall that transcends sessions
- Semantic similarity matching (not exact keyword)
- RAG-style context augmentation

---

## Layer 3: Working Memory (Cache)

**Technology:** Redis (local key-value store)  
**Location:** localhost:6379  
**Status:** ✅ Running locally

### How It Works
- Fast in-memory KV cache
- Session state, temporary context
- Sub-second access for working memory

### Status
```bash
redis-cli ping  # Returns PONG
redis-server --daemonize yes  # Already running
```

### When to Use
- Fast session state storage
- Temporary context that doesn't need persistence
- Cross-turn working memory (faster than file I/O)

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
│              │   Memory Router      │        │
│              │   (schema-based)     │        │
│              └───────────┬───────────┘        │
└──────────────────────────┼───────────────────┘
                           │
      ┌────────────────────┼────────────────────┐
      ▼                    ▼                    ▼
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   CHROMA    │      │   REDIS     │      │   OTHERS    │
│  (Vector)   │      │  (KV Cache) │      │ (mem0, etc) │
│             │      │             │      │             │
│ Semantic    │      │ Fast working│      │ Pluggable   │
│ recall, RAG │      │ memory      │      │             │
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
| `mem0_search` | External | Semantic search (if mem0 configured) |
| `mem0_profile` | External | Full user profile |
| `mem0_conclude` | External | Store durable fact |
| Custom tools | Chroma/Redis | Access vector/KV layers as needed |

---

## Key Points for Rae

1. **File memory is always on** — no config needed, used by default
2. **Chroma is available** — use when you need semantic similarity search
3. **Redis is running** — use for fast working memory/caching
4. **Don't overthink** — start with file memory, add layers as needed
5. **80% rule** — consolidate when memory gets full
6. **Schema is stable** — backends can change, tools stay the same

---

## Future Possibilities

- Neo4j for graph-based memory (entity relationships)
- Mem0 cloud for advanced embeddings
- Custom plugin for specific use cases

The architecture is modular — new backends plug in via the same interface.