---
name: kimi-claude-code-setup
description: Setup Claude Code to use Kimi For Coding API as backend — install, configure, and common pitfalls.
---

# Kimi API + Claude Code Setup

Use Kimi's coding API as the backend for Claude Code. Kimi's API is Anthropic-compatible and accepts standard Claude model names.

## 1. Install Claude Code

```bash
# If npm certificate issues arise:
npm config set strict-ssl false

npm install -g @anthropic-ai/claude-code
```

Run with `npx claude` if the `claude` binary isn't in PATH.

## 2. Configuration (Critical)

**The most important rule: `ANTHROPIC_BASE_URL` must NOT end with `/v1`.** Claude Code SDK auto-appends `/v1`, so including it causes double `/v1/v1` → 404 on model validation.

| Setting | Value |
|---------|-------|
| `ANTHROPIC_BASE_URL` | `https://api.kimi.com/coding` (NO `/v1`) |
| `ANTHROPIC_API_KEY` | Your `sk-kimi-...` key |
| Model | `kimi-for-coding` or `claude-sonnet-4-6` (both work, same backend) |

### Quick one-liner:

```bash
ANTHROPIC_API_KEY="sk-kimi-..." ANTHROPIC_BASE_URL="https://api.kimi.com/coding" npx claude -p "prompt" --model kimi-for-coding
```

### Convenience script (`~/问kimi.sh`):

```bash
#!/bin/bash
export ANTHROPIC_API_KEY="sk-kimi-..."
export ANTHROPIC_BASE_URL="https://api.kimi.com/coding"
npx claude -p "$*" --model kimi-for-coding
```

Usage: `~/问kimi.sh "你的问题"`

## 3. Pitfalls

- **OpenCode does NOT work** with Kimi For Coding API. Kimi blocks non-whitelisted agents (only allows: Claude Code, Kimi CLI, Roo Code, Kilo Code).
- **`ANTHROPIC_BASE_URL` with `/v1` suffix** → model 404, confusing error "There's an issue with the selected model". Fix: strip `/v1`.
- **npm certificate issues**: `npm config set strict-ssl false` before install.
- Model name `kimi-for-coding` works even though Claude Code doesn't have it in its local list — the API validates it remotely.
