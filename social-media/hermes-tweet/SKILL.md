---
name: hermes-tweet
description: "Use Hermes Tweet for X/Twitter drafting, live reads, monitoring, and explicitly approved account actions from Hermes Agent."
version: 1.0.0
author: Hermes Tweet contributors
license: MIT
platforms: [linux, macos, windows]
prerequisites:
  commands: [hermes]
metadata:
  hermes:
    tags: [x, twitter, social-media, hermes-plugin, drafting, monitoring]
    homepage: https://github.com/Xquik-dev/hermes-tweet
---

# Hermes Tweet

Use Hermes Tweet when the user needs X/Twitter research, post drafting,
thread planning, reply preparation, live read checks, or account actions from
Hermes Agent.

## Install

```bash
hermes plugins install Xquik-dev/hermes-tweet --enable
```

If a previous install is disabled, run `hermes plugins enable hermes-tweet`.
Set `XQUIK_API_KEY` before live reads. If Hermes was already running, use
`/reload` in an interactive session or restart gateway and cron sessions.

Without an API key, Hermes exposes only the no-network `tweet_explore` tool.
Set `HERMES_TWEET_ENABLE_ACTIONS=true` only in sessions where the user needs
private or account-changing actions.

## Tools

- Use `tweet_explore` to search the bundled endpoint catalog without a network
  call or API key.
- Use `tweet_read` for catalog-listed read-only endpoints.
- Use `tweet_action` for private or write-like endpoints only after explicit
  user approval. This tool stays disabled by default.

## Use

- Draft posts, replies, quote posts, and threads in draft-only mode by default.
- Use live reads only when current X/Twitter context changes the answer.
- Show the final text, target account or post, and action before any write.
- Wait for explicit user confirmation before posting, replying, following,
  liking, reposting, deleting, or changing account state.
- Never ask users to paste API keys, cookies, tokens, or session values into
  chat.

If the API key is missing, continue with planning and drafting only. Do not
simulate live X/Twitter state.

Xquik is an independent third-party service. Not affiliated with X Corp. "Twitter" and "X" are trademarks of X Corp.
