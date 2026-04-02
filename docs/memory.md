# Memory Architecture

Jarvis maintains continuity across sessions through a layered memory system. This is what separates it from a stateless chatbot.

---

## Memory Layers

### Layer 1 — Daily Logs (`memory/YYYY-MM-DD.md`)

**Purpose:** Raw session notes — what happened today  
**Written by:** The agent during and after sessions  
**Format:** Freeform Markdown, timestamped entries  
**Retention:** Keep for ~30 days, then archive or summarize

Example:
```markdown
# 2026-04-01

- Pushed 5 GitHub repos for ML portfolio
- User asked to check CI on sign-language-translator PR
- Discussed Stanford application strategy — user wants to highlight RAG pipeline
```

---

### Layer 2 — Long-term Memory (`MEMORY.md`)

**Purpose:** Curated, distilled facts worth remembering indefinitely  
**Written by:** The agent (during heartbeats, when explicitly asked)  
**Format:** Structured Markdown — preferences, facts, decisions, lessons  
**Retention:** Indefinite; review and prune outdated entries periodically

This file is the agent's "brain" — the equivalent of a human's long-term episodic and semantic memory.

Example contents:
```markdown
## User Preferences
- Prefers concise responses without filler phrases
- Dark mode everywhere
- Uses VSCode with Vim keybindings

## Projects
- sign-language-translator: GitHub Infinty-ux, real MediaPipe + RF classifier
- Applying to MIT/Stanford/CMU, ML/AI track

## Lessons Learned
- PowerShell doesn't support && chaining — use separate exec calls
```

**Security note:** `MEMORY.md` is only loaded in the main session (direct chat). It is never loaded in shared contexts or Discord servers with other users.

---

### Layer 3 — Semantic Search (`memory_search`)

**Purpose:** Find relevant memories without knowing exactly where they are  
**How it works:** Vector similarity search across all `memory/*.md` + `MEMORY.md`  
**When used:** At the start of any session that might benefit from prior context

```
memory_search("RAG pipeline project") 
→ returns top-k snippets with file + line numbers
```

---

## Memory Maintenance

During heartbeats, the agent:

1. Reads recent `memory/YYYY-MM-DD.md` files
2. Identifies significant events, decisions, lessons
3. Updates `MEMORY.md` with distilled insights
4. Prunes stale entries from long-term memory

This mimics how humans consolidate short-term into long-term memory during sleep.

---

## Writing to Memory

The agent writes to memory files automatically, but you can also trigger it:

- *"Remember that I prefer X"* → writes to `MEMORY.md`
- *"Note that the deployment uses port 8080"* → writes to today's log
- *"Summarize what we worked on this week"* → reads logs + generates summary

---

## Privacy

- `MEMORY.md` contains personal context and is never shared externally
- Daily logs are local files, never uploaded
- The agent does not send memory contents to external services (only the AI model via the tool call API)
