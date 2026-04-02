# Jarvis — Personal AI Agent

A persistent, self-aware personal AI agent built on [**OpenClaw**](https://openclaw.dev), connected to Discord, with full shell execution capabilities, long-term memory, a skill system, and real-time tool access.

This isn't a chatbot wrapper. It's an autonomous agent that lives on your machine, knows who you are, remembers what matters, and gets things done.

---

## What It Is

Jarvis is a personal AI agent that:

- Connects to **Discord** as a bot — respond to messages, react, participate in servers
- Runs shell commands (`exec`) directly on your machine — write files, run scripts, call APIs
- Maintains **long-term memory** across sessions — no starting from scratch every time
- Learns from a **skill system** — specialized instruction files that extend its capabilities
- Operates on a **heartbeat** — periodically checks email, calendar, weather without being asked
- Spawns **subagents** for parallel workloads — push-based result delivery, no polling

---

## Architecture

```
Discord (channel)
        │
        ▼
OpenClaw Gateway (Node.js daemon)
        │
        ├── Session Manager
        │     ├── Main session (persistent, has MEMORY.md)
        │     └── Subagent sessions (ephemeral, task-scoped)
        │
        ├── Tool Layer
        │     ├── exec / process    → shell access
        │     ├── read / write / edit → file operations
        │     ├── web_search / web_fetch → internet access
        │     ├── image             → vision model
        │     └── memory_search / memory_get → semantic memory
        │
        ├── Skill System
        │     └── ~/AppData/Roaming/npm/node_modules/openclaw/skills/
        │           ├── github/     → gh CLI operations
        │           ├── weather/    → wttr.in / Open-Meteo
        │           ├── discord/    → Discord-specific ops
        │           ├── clawflow/   → multi-step task orchestration
        │           └── ...
        │
        └── Workspace
              ├── MEMORY.md         → long-term curated memory
              ├── SOUL.md           → personality & behavior config
              ├── USER.md           → who the user is
              ├── TOOLS.md          → environment-specific notes
              └── memory/YYYY-MM-DD.md → daily session logs
```

---

## Capabilities

### 🧠 Memory

- **Session memory:** Every conversation is aware of prior context via daily log files
- **Long-term memory:** `MEMORY.md` is a curated, human-edited-quality record of important facts, preferences, and history
- **Semantic search:** `memory_search` uses vector similarity to find relevant memories across all files
- **Self-updating:** The agent writes to its own memory files — no manual journaling needed

### 🛠️ Tool Access

| Tool | What it does |
|---|---|
| `exec` | Run any shell command — PowerShell, bash, Python, git, gh, etc. |
| `read/write/edit` | Full filesystem access within the workspace |
| `web_search` | DuckDuckGo search with snippets |
| `web_fetch` | Fetch and extract page content as Markdown |
| `image` | Analyze images with a vision model |
| `memory_search` | Semantic search across all memory files |
| `process` | Manage background sessions, send keystrokes, poll output |

### ⚡ Skills

Skills are instruction files (`SKILL.md`) that specialize the agent for specific domains:

- **`github`** — create issues, review PRs, check CI status via `gh` CLI
- **`weather`** — get forecasts from wttr.in / Open-Meteo
- **`discord`** — channel operations, reactions, message management
- **`clawflow`** — multi-step job orchestration with persistent state
- **`healthcheck`** — security auditing and hardening
- **`skill-creator`** — write new skills from scratch

### 💓 Heartbeat

The agent wakes up periodically (configurable interval) and proactively:
- Checks unread email
- Scans upcoming calendar events
- Checks weather if you might go out
- Reviews and updates memory files
- Runs any tasks listed in `HEARTBEAT.md`

### 🤖 Subagents

Complex or long-running tasks can be delegated to **subagents** — ephemeral parallel sessions that:
- Run in isolation from the main session
- Push results back automatically when done
- Can be labeled for identification (`subagents.label`)
- Support nested depth for task hierarchies

---

## Setup

See [`docs/setup.md`](docs/setup.md) for full installation instructions.

Quick start:
```bash
npm install -g openclaw
openclaw init
openclaw gateway start
```

---

## Files

```
jarvis-ai-agent/
├── README.md           ← this file
└── docs/
    ├── setup.md        ← installation & configuration
    ├── skills.md       ← skill system reference
    ├── memory.md       ← memory architecture
    └── tools.md        ← tool reference
```

---

## Why It Matters

Most "AI assistants" are stateless — they forget everything after the conversation ends. Jarvis is different:

- It **remembers** your preferences, projects, and history
- It **acts** — writes code, pushes to GitHub, sends messages
- It **monitors** — proactively checks things you care about
- It **learns** — the skill system means its capabilities grow over time

This is what personal AI looks like when you move beyond chat.
