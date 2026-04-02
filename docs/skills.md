# Skills Reference

Skills are instruction files (`SKILL.md`) that extend the agent's capabilities for specific domains. The agent scans available skills before responding and loads the most relevant one automatically.

## How Skills Work

1. Each skill lives in `~/.openclaw/skills/<name>/SKILL.md`
2. The agent reads skill `<description>` entries at startup
3. When a task matches a skill's description, the agent loads and follows it
4. Skills can reference helper scripts in `scripts/` or reference docs in `references/`

## Built-in Skills

### `github`
**When to use:** Checking PR status, creating issues, reviewing CI runs, querying the GitHub API.

Capabilities:
- Create, list, and comment on issues and PRs
- Check CI/CD run status and fetch logs
- Query the GitHub API via `gh api`
- Review PR diffs and suggest changes

Example trigger: *"Check if CI passed on my latest PR"*

---

### `weather`
**When to use:** Current conditions, forecasts, temperature queries for any location.

Sources: wttr.in (no API key), Open-Meteo (free tier)

Example trigger: *"What's the weather in NYC this weekend?"*

---

### `discord`
**When to use:** Discord-specific operations — sending to channels, managing messages, reactions.

Example trigger: *"Send a summary to #general"*

---

### `clawflow`
**When to use:** Multi-step jobs that need flow identity, waiting on external events, or structured output delivery.

Features:
- Persistent job context across subagent hops
- Wait on outside triggers (webhooks, timers)
- Structured progress reporting

Example trigger: *"Process these 50 documents in parallel and summarize results"*

---

### `clawflow-inbox-triage`
**When to use:** Message triage where different intents need different routing — some notify immediately, some wait, others batch into summaries.

---

### `healthcheck`
**When to use:** Security audits, firewall/SSH hardening, risk posture reviews, OpenClaw version checks.

Capabilities:
- Firewall rule audit
- SSH config hardening review
- Automatic update status check
- Risk tolerance configuration

Example trigger: *"Run a security audit on this machine"*

---

### `node-connect`
**When to use:** Diagnosing OpenClaw node pairing failures — QR code issues, mobile app connection problems, Tailscale errors.

---

### `skill-creator`
**When to use:** Creating new skills from scratch, improving existing SKILL.md files, auditing skill structure.

Example trigger: *"Create a skill for Notion API operations"*

---

## Creating Custom Skills

```bash
openclaw skills create my-skill
```

Or ask the agent: *"Create a skill for [domain]"* — it will use the `skill-creator` skill to author it.

Skill file structure:
```
skills/my-skill/
├── SKILL.md          ← main instruction file (required)
├── scripts/          ← helper scripts (optional)
└── references/       ← reference docs (optional)
```

Minimal `SKILL.md`:
```markdown
# My Skill

## When to use
<description of trigger conditions>

## Instructions
<step-by-step instructions for the agent>
```
