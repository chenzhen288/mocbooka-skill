---
name: ollama-macos-13-install
description: Install Ollama on macOS 13 (Ventura) — latest version incompatible, need older release. Also handles proxy downloads.
---

# Ollama on macOS 13 (Ventura)

Latest Ollama (v0.5+) requires macOS 14+ (Sonoma). Brew also fails because mlx dependency needs Xcode 15 and Sonoma.

## Install

```bash
# 1. Download older compat version (v0.3.14 works on Ventura)
curl -x http://127.0.0.1:17891 -L -o /tmp/Ollama.zip \
  "https://github.com/ollama/ollama/releases/download/v0.3.14/Ollama-darwin.zip"

# 2. Extract and open
unzip -o /tmp/Ollama.zip -d /tmp/ollama
open /tmp/ollama/Ollama.app
```

## Verify

```bash
sleep 5 && curl http://127.0.0.1:11434/api/tags
# Should return: {"models":[]}
```

## Pull model

```bash
ollama pull qwen3:0.6b
```

## Pitfalls

- Latest Ollama: `kLSIncompatibleSystemVersionErr` on macOS 13
- Brew: mlx needs Xcode 15 + Sonoma
- Download from ollama.com directly may be slow; use GitHub releases mirror or local proxy
- User (周芷若) proxy: 127.0.0.1:17891 (Clash/similar)
