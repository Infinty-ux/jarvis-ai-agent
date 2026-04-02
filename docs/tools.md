# Tools Reference

Jarvis has access to a set of first-class tools. These are not plugins — they are native capabilities exposed directly to the model.

---

## File Operations

### `read`
Read a file's contents (text or image). Supports pagination via `offset`/`limit` for large files.

```
read("~/project/src/model.py", offset=50, limit=100)
```

### `write`
Create or overwrite a file. Automatically creates parent directories.

```
write("~/workspace/notes.md", content="...")
```

### `edit`
Make precise targeted edits using exact string replacement. Faster and safer than rewriting entire files.

```
edit("config.py", edits=[{oldText: "debug=True", newText: "debug=False"}])
```

---

## Shell Execution

### `exec`
Run shell commands on the host machine.

Options:
- `pty=true` — allocate a pseudo-terminal (for interactive CLIs)
- `background=true` — run without waiting
- `yieldMs` — background after N milliseconds
- `elevated=true` — run with elevated permissions (requires approval)
- `timeout` — kill after N seconds

```
exec("python src/trainer.py --epochs 30", workdir="~/ml-project", yieldMs=60000)
```

### `process`
Manage background exec sessions — list, poll output, send keystrokes, kill.

Actions: `list`, `poll`, `log`, `write`, `send-keys`, `kill`

```
process(action="poll", sessionId="abc123", timeout=5000)
```

---

## Web

### `web_search`
DuckDuckGo search. Returns titles, URLs, snippets.

```
web_search("EfficientNet-B4 deepfake detection benchmark", count=5)
```

### `web_fetch`
Fetch a URL and extract readable content as Markdown or text.

```
web_fetch("https://arxiv.org/abs/2104.02610", extractMode="markdown")
```

---

## Vision

### `image`
Analyze one or more images with a vision model. Use for screenshots, diagrams, photos.

```
image(image="screenshot.png", prompt="What error is shown?")
```

---

## Memory

### `memory_search`
Semantic search across all memory files. Returns top-k snippets with file path + line numbers.

```
memory_search("Stanford application essay topics", maxResults=5)
```

### `memory_get`
Read specific lines from a memory file — use after `memory_search` to avoid loading entire files.

```
memory_get("memory/2026-04-01.md", from=10, lines=20)
```

---

## Session Management

### `sessions_yield`
End the current turn and hand control back to the requester. Used by subagents to signal completion.

---

## Tool Policies

- **Approval required** for elevated shell commands (`exec` with `elevated=true`)
- **Approval cards** appear in Discord when elevated commands are requested — use the card buttons, not manual `/approve` in chat
- **Rate limiting** — the agent prefers fewer larger operations over tight loops
- **Safety** — destructive operations (deletions, rewrites) are narrated before execution

---

## Adding Tool Notes

If your setup has specific values (SSH hosts, camera names, API endpoints), document them in `TOOLS.md` in the workspace:

```markdown
### SSH Hosts
- homeserver → 192.168.1.100, user: ubuntu

### Local APIs
- ML inference server → http://localhost:8080

### Devices
- webcam → index 0 (front), index 1 (USB external)
```

The agent reads `TOOLS.md` at session start and uses these details when operating tools.
