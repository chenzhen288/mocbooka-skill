---
name: hermes-kimi-config
description: 将 Hermes 默认模型切换到 Kimi（kimi-k2.6），通过 kimi-coding-cn provider。包含常见坑和解决步骤。
version: 1.0.0
author: 周芷若
---

# Hermes 切到 Kimi 模型

把 Hermes 默认大脑换成 Kimi（kimi-k2.6）。

## 一句话

改 `~/.hermes/config.yaml`，provider 用 `kimi-coding-cn`，模型用 `kimi-k2.6`。

## 步骤

### 1. 改配置文件

编辑 `~/.hermes/config.yaml`，改这三个地方：

```yaml
model:
  default: kimi-k2.6
  provider: kimi-coding-cn

kimi-coding-cn:
  api_key: sk-kimi-你的key
  base_url: https://api.kimi.com/coding/v1
```

### 2. 删掉旧的 moonshot 配置（如果有）

如果配置文件底部还有 `moonshot:` 段，删掉它。

### 3. 验证

```bash
hermes doctor
```

看到 `Kimi / Moonshot (China): ✓` 就是通了。

## 坑

| 错误 | 原因 | 解决 |
|------|------|------|
| `hermes model set` 报交互式错误 | 这个命令只能在真实终端跑，不能从脚本/管道执行 | 直接改 config.yaml |
| provider `moonshot` 不识别 | Hermes 没有 `moonshot` 这个 provider | 用 `kimi-coding-cn` |
| provider `kimi-coding` HTTP 402 | 国际版 API 付费限制，免费 key 过不去 | 用 `kimi-coding-cn`（中国版） |
| 模型名别加前缀 | `moonshotai/kimi-k2.6` 会报警告 | 直接用 `kimi-k2.6` |

## API Key

Kimi 会员 key（`sk-kimi-` 开头），从 Kimi For Coding 获取。
