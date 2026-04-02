# Setup Guide

## Prerequisites

- Node.js v18+ (v20+ recommended)
- npm
- A Discord account and a bot token
- PowerShell (Windows) or bash (macOS/Linux)

---

## 1. Install OpenClaw

```bash
npm install -g openclaw
```

Verify:
```bash
openclaw --version
```

---

## 2. Initialize workspace

```bash
openclaw init
```

This creates `~/.openclaw/workspace/` with starter files:
- `SOUL.md` — personality config (edit to match your preferences)
- `USER.md` — information about you (fill this in)
- `TOOLS.md` — environment-specific notes
- `AGENTS.md` — agent behavior rules

---

## 3. Configure Discord bot

### Create a Discord application

1. Go to [discord.com/developers/applications](https://discord.com/developers/applications)
2. Click **New Application** → name it (e.g. "Jarvis")
3. Go to **Bot** tab → click **Add Bot**
4. Under **Token**, click **Reset Token** and copy it
5. Enable **Message Content Intent** under Privileged Gateway Intents

### Invite to your server

1. Go to **OAuth2 → URL Generator**
2. Select scopes: `bot`, `applications.commands`
3. Select permissions: `Send Messages`, `Read Message History`, `Add Reactions`, `Attach Files`
4. Copy the URL and open it in a browser → invite to your server

### Configure OpenClaw

```bash
openclaw config set discord.token "YOUR_BOT_TOKEN"
openclaw config set discord.channels "CHANNEL_ID_1,CHANNEL_ID_2"
```

Find channel IDs: Right-click a channel in Discord → **Copy Channel ID** (enable Developer Mode in Settings → Advanced first).

---

## 4. Configure AI model

OpenClaw supports multiple providers:

```bash
openclaw config set model.provider anthropic
openclaw config set model.apiKey "YOUR_ANTHROPIC_API_KEY"
openclaw config set model.name "claude-sonnet-4-6"
```

Supported providers: `anthropic`, `openai`, `openrouter`

---

## 5. Start the gateway

```bash
openclaw gateway start
```

The gateway runs as a background daemon. Verify it's running:
```bash
openclaw gateway status
```

---

## 6. Customize your agent

Edit the workspace files to personalize the agent:

**`~/.openclaw/workspace/SOUL.md`** — Personality, tone, behavior rules  
**`~/.openclaw/workspace/USER.md`** — Your name, timezone, preferences, goals  
**`~/.openclaw/workspace/TOOLS.md`** — SSH hosts, camera names, device nicknames  

---

## 7. Heartbeat (optional)

To enable proactive background checks:

```bash
openclaw config set heartbeat.enabled true
openclaw config set heartbeat.intervalMinutes 30
openclaw config set heartbeat.channel CHANNEL_ID
```

Create `~/.openclaw/workspace/HEARTBEAT.md` with a checklist of things to monitor.

---

## Gateway commands

```bash
openclaw gateway start    # start daemon
openclaw gateway stop     # stop daemon
openclaw gateway restart  # restart
openclaw gateway status   # check status + active sessions
```

---

## Troubleshooting

**Bot not responding in Discord:**
- Verify Message Content Intent is enabled in Discord Developer Portal
- Check channel IDs are correct
- Run `openclaw gateway status` to confirm daemon is running

**Permission errors on exec:**
- The agent can request elevated permissions for individual commands
- Review and approve via Discord approval cards

**Memory not persisting:**
- Check `~/.openclaw/workspace/` is writable
- Daily logs go to `memory/YYYY-MM-DD.md`
- Long-term memory is in `MEMORY.md`
