---
name: 3agent-mcp-architecture
description: Deploy a 3-agent MCP architecture — Hermes as boss/planner, OpenClaw as executor, CLaudex as coder. Connected via MCP protocol layer. Hermes handles task decomposition and decision-making only; delegates execution and coding to sub-agents. Prevents single-agent bottleneck and dramatically improves stability.
version: 1.0.0
author: User
license: MIT
metadata:
  hermes:
    tags: [multi-agent, mcp, hermes, openclaw, claudex, architecture, boss-worker]
    related_skills: [hermes-agent, opencode, native-mcp]
---

# 3-Agent MCP Architecture

A production-ready multi-agent setup where **Hermes acts as the boss** (主脑/联络窗口), handling task decomposition, routing decisions, result validation, and real-time monitoring (step/time tracking) — while **OpenClaw (小龙虾)** and **CLaudex** handle execution and coding respectively. All communication goes through an **MCP protocol layer**.

## Why This Architecture

| Problem (Single Agent) | Solution (3-Agent) |
|---|---|
| Hermes gets stuck on coding tasks | Hermes never codes; only plans and delegates |
| One agent context gets polluted | Each agent has focused, isolated responsibility |
| Slow on mixed tasks | Parallel delegation, specialized execution |
| High failure/crash rate | Separation of concerns = higher stability |
| No visibility into execution | Real-time step/time tracking via Hermes |

## Architecture Diagram

```
周芷若 (User)
  ↓ 直接对话
Hermes (主脑 / 联络窗口)
  ├─ 任务拆解 (Task Decomposition)
  ├─ 路由决策 (Routing Decision)
  ├─ 结果校验 (Result Validation)
  ├─ 实时监控 — 步骤/耗时追踪
  ↓ 通过 MCP 协议层调度
  ├─→ OpenClaw (小龙虾 / 执行层) — 文件操作、系统命令、轻量任务、快速响应
  └─→ CLaudex (编码层) — 复杂编码、架构设计、代码审查、代码重构
  ↓ 汇总结果
周芷若
```

## Agent Responsibilities

### Hermes (Boss)
- **唯一与用户对话的窗口**
- 接收任务，拆解为子任务
- 决定调用哪个子agent (OpenClaw or CLaudex)
- 必要时做 AutoPlan 解释，生成执行步骤
- 汇总子agent结果，校验后返回给用户
- **不直接执行代码、不写代码**

### OpenClaw / 小龙虾 (执行层)
- 文件操作
- 系统命令
- 轻量任务
- 快速响应

### CLaudex (编码层)
- 复杂编码
- 架构设计
- 代码审查
- 代码重构

## Deployment Steps

### Step 1: Install Hermes

```bash
# Install Hermes Agent
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# Verify
hermes doctor
```

### Step 2: Configure Hermes as Boss

Edit `~/.hermes/config.yaml` or use commands:

```bash
# Set Hermes model (should be good at planning/decomposition)
hermes model

# Enable delegation toolset
hermes tools enable delegation

# Enable MCP
hermes mcp add openclaw --command "openclaw"
hermes mcp add claudex --command "claudex"
```

**Critical Prompt for Hermes:**

Tell Hermes explicitly:

> "你是一个主脑（Boss/联络窗口）。你的职责是：
> 1. 接收用户任务，拆解为子任务
> 2. 做路由决策，决定派发给哪个子agent
> 3. 实时监控执行步骤和耗时
> 4. 将文件操作、系统命令、轻量任务、快速响应类任务委派给 OpenClaw (小龙虾)
> 5. 将复杂编码、架构设计、代码审查、代码重构类任务委派给 CLaudex
> 6. 汇总结果，校验后返回给用户
> 
> **禁止自己执行代码或写代码**。你必须通过 MCP 调用子agent来干活。"

### Step 3: Deploy OpenClaw (via Hermes)

Ask Hermes to deploy OpenClaw:

```
你知道 OpenClaw (小龙虾) 吗？帮我部署一个 OpenClaw 实例作为执行层。
```

Hermes should:
1. Install `openclaw` CLI
2. Configure it as an MCP server
3. Test connectivity

Verify:
```bash
hermes mcp list
hermes mcp test openclaw
```

### Step 4: Deploy CLaudex (via Hermes + OpenClaw)

CLaudex is the coding agent. User originally tried `claude` (Korean rewrite) but it only supported SiliconFlow API and couldn't use third-party APIs.

**Recommended: CLaudex** (支持 NVIDIA 免费 API)

Ask Hermes:

```
部署 CLaudex 作为编码层。使用 NVIDIA 的免费 API，通过 SiliconFlow 获取 API key。
```

Configuration:
- API: NVIDIA NIM (via SiliconFlow)
- Model: DeepSeek-Coder-V2 / Qwen2.5-Coder / similar
- API Key: 从 SiliconFlow 获取免费额度

Verify:
```bash
hermes mcp list
hermes mcp test claudex
```

### Step 5: Test Collaboration

Test the full pipeline:

```
周芷若: "帮我搭建一个 FastAPI 用户认证服务"

Hermes:
  1. 拆解任务:
     - 子任务A: 搭建 FastAPI 项目结构 (委派给 CLaudex)
     - 子任务B: 实现 JWT 认证中间件 (委派给 CLaudex)
     - 子任务C: 部署并测试服务 (委派给 OpenClaw)
  2. [实时监控] 步骤 1/3: CLaudex 编码中... 耗时 2min
  3. [实时监控] 步骤 2/3: CLaudex 编码完成
  4. [实时监控] 步骤 3/3: OpenClaw 部署测试中... 耗时 1min
  5. 汇总结果，返回给周芷若
```

## MCP Configuration

### Hermes MCP Server List

```yaml
# ~/.hermes/config.yaml 中的 mcp 部分
mcp:
  servers:
    openclaw:
      command: "openclaw"
      args: ["--mcp", "--stdio"]
      env:
        OPENCLAW_API_KEY: "${OPENCLAW_API_KEY}"
    
    claudex:
      command: "claudex"
      args: ["--mcp", "--stdio"]
      env:
        SILICONFLOW_API_KEY: "${SILICONFLOW_API_KEY}"
        NVIDIA_API_KEY: "${NVIDIA_API_KEY}"
```

### MCP Protocol Layer

- Hermes 通过 `delegate_task` 或 `hermes mcp` 调用子agent
- 子agent之间不直接通信，都通过 Hermes 调度
- 每个子agent有独立的 session、tools、environment

## API Key Setup

### NVIDIA Free API (via SiliconFlow)

1. 注册 [SiliconFlow](https://siliconflow.cn)
2. 获取免费 API Key
3. 配置到 `~/.hermes/.env`:

```bash
SILICONFLOW_API_KEY=sf-your-key-here
NVIDIA_API_KEY=${SILICONFLOW_API_KEY}  # alias
```

### OpenCode API Key

```bash
OPENCODE_API_KEY=your-opencode-key
```

## Troubleshooting

### CLaudex 只能使用 SiliconFlow API

**问题**: 某些 claude 实现只支持 SiliconFlow，不支持其他第三方 API。
**解决**: 换用 CLaudex，它支持 NVIDIA NIM API，兼容性更好。

### Hermes 自己开始写代码

**问题**: Hermes 没有正确进入 "Boss 模式"，自己执行了代码任务。
**解决**: 
1. 重新强调 prompt: "你是主脑，只规划不执行"
2. 检查 `delegation` toolset 是否启用
3. 在 Hermes 的 system prompt 中加入角色限制

### MCP 连接失败

```bash
# 测试 MCP 连接
hermes mcp test openclaw
hermes mcp test claudex

# 检查进程是否在运行
ps aux | grep -E "openclaw|claudex"

# 重启 MCP
hermes mcp remove openclaw
hermes mcp add openclaw --command "openclaw"
hermes mcp remove claudex
hermes mcp add claudex --command "claudex"
```

### 子agent返回结果不完整

**解决**: 在 Hermes 的 prompt 中加入:
> "收到子agent结果后，必须校验完整性。如果缺失，要求重新执行。"

## Tips

- **Hermes 模型选择**: 选擅长规划和长上下文理解的模型 (Claude Sonnet, DeepSeek-V3, Qwen-Max)
- **OpenClaw 模型选择**: 选通用能力强的模型 (GPT-4o, Claude Sonnet)
- **CLaudex 模型选择**: 选代码专用模型 (DeepSeek-Coder-V2, Qwen2.5-Coder)
- **Worktree 模式**:  spawning 子agent时加 `-w` 参数，避免 git 冲突
- **监控成本**: 3-agent 架构调用次数更多，注意 API 费用监控

## Variations

### 2-Agent (简化版)

如果 CLaudex 部署困难，可以只用 Hermes + OpenClaw:
- Hermes: 规划 + 轻量编码审查
- OpenClaw: 执行 + 编码

### 4-Agent (扩展版)

增加专用 agent:
- Hermes: Boss
- OpenClaw: 执行
- CLaudex: 编码
- **Tester**: 专门做测试和 QA

## Verification Checklist

- [ ] Hermes 安装完成，可正常对话
- [ ] Hermes 明确知道自己是 "Boss" 角色（只做路由决策，不执行）
- [ ] OpenClaw MCP 连接成功 (`hermes mcp test openclaw`)
- [ ] CLaudex MCP 连接成功 (`hermes mcp test claudex`)
- [ ] 端到端测试: 给一个任务，Hermes 能拆解并委派
- [ ] 子agent结果能正确返回给 Hermes
- [ ] Hermes 能校验并汇总结果返回给周芷若
- [ ] 实时监控正常: 步骤和耗时追踪可见

## References

- Hermes Agent Docs: https://hermes-agent.nousresearch.com/docs/
- MCP Protocol: https://modelcontextprotocol.io
- OpenClaw: https://github.com/openclaw
- SiliconFlow: https://siliconflow.cn
- NVIDIA NIM: https://build.nvidia.com/explore/discover
