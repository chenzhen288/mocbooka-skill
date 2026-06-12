---
name: macos-proxy-download
description: Download files through local proxy on macOS — detect proxy settings, use them for blocked-content downloads.
---

# macOS Proxy Download

When downloads from blocked sites (ollama.com, GitHub releases, etc.) fail with SSL errors or timeout, check if the Mac has a local proxy and use it.

## 1. Detect Proxy

```bash
# Check env vars
echo "http_proxy=$http_proxy"
echo "https_proxy=$https_proxy"

# Check macOS system proxy (Wi-Fi)
networksetup -getwebproxy Wi-Fi
networksetup -getsecurewebproxy Wi-Fi
```

If `Enabled: Yes`, note the `Server` and `Port` (typically `127.0.0.1:PORT` for Clash/V2Ray).

## 2. Download Through Proxy

```bash
curl -x http://127.0.0.1:PORT -L -o /path/output "https://blocked.url/file"
```

For background downloads with auto-notify:
```bash
curl -x http://127.0.0.1:PORT -L --connect-timeout 15 --max-time 300 -o /path/output "URL" && echo "done"
```

## 3. Pitfalls

- **brew install ollama** fails on macOS 13 (Ventura) — needs Xcode 15 + Sonoma. Use direct download instead.
- **curl without proxy** times out or gets SSL_ERROR_SYSCALL when the site is blocked.
- Proxy port may vary per tool (Clash default: 7890, some configs: 17891). Always check actual settings.
