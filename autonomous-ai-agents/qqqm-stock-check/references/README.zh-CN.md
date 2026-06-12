# QQQM 股价查询工具

一键查美股，主打一个"傻瓜式"。

## 一句话说明

打 `qqqm` 就出来 QQQM 当前价格，就这么简单。

## 怎么装

已经装好了，直接用。

```bash
qqqm    # 查 QQQM
```

## 能干嘛

- ✅ 查 QQQM 实时价格
- ✅ 显示涨跌额、涨跌幅
- ✅ 显示最近交易日

## 不能干嘛（目前）

- ❌ 查别的股票（后续会加）
- ❌ 价格提醒
- ❌ 自动监控

## 文件在哪

```
/Users/zhenchen/py文件夹/qqqm_check.py    ← 脚本本体
~/.local/bin/qqqm                          ← 快捷命令（软链接）
```

## API 用的是谁

[Alpha Vantage](https://www.alphavantage.co/)，免费额度够用。
内置 Key：`55FBB93ER7GZFA6T`

## 常见问题

**Q: 为啥不能查别的股票？**
A: 目前只写了 QQQM，后续可以加。

**Q: 数据延迟多久？**
A: Alpha Vantage 免费版大概延迟几分钟到十几分钟，够用了。

**Q: 想换自己的 API Key？**
A: 改脚本里的 `api_key` 变量就行。
