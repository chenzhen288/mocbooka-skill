---
name: qqqm-stock-check
description: 一键查询 QQQM 及美股实时行情，Alpha Vantage 接口
version: 1.0.0
author: 周芷若
---

# QQQM 美股查询 Skill

一键查 QQQM 或其他美股实时价格。

## 安装

```bash
# 已内置 alias/命令
qqqm          # 查 QQQM
qqqm A剑指苍穹  # 查其他股票
```

## 用法

| 命令 | 说明 |
|------|------|
| `qqqm` | 查 QQQM 当前价格 |
| `python3 /Users/zhenchen/py文件夹/qqqm_check.py` | 直接运行脚本 |

## API Key

- 默认使用 Alpha Vantage 免费接口
- Key 已内置: `55FBB93ER7GZFA6T`
- 如需自己的 key，设置环境变量：`export ALPHA_VANTAGE_API_KEY=你的key`

## 进阶

脚本位置: `/Users/zhenchen/py文件夹/qqqm_check.py`
可修改支持多股票查询、价格提醒等功能。
