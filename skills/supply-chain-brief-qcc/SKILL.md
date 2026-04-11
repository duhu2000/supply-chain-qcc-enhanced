---
name: supply-chain-brief-qcc
description: >
  适用于：供应链简报、每周供应链报告、采购官报告、
  运营官报告、采购仪表板、供应链仪表板、每周报告、
  高管简报、供应链KPI、供应链指标。

  **企查查MCP增强版**：在每周简报中包含中国供应商风险摘要
  用于主动风险管理。

  不适用于：详细供应商评估（使用vendor-assessment-qcc）、详细
  发票对账（使用invoice-reconciliation-qcc）。
license: Apache-2.0
metadata:
  author: Panaversity (企查查MCP增强版)
  version: "2.0"
  plugin-commands: "/supply-chain-brief-qcc"
  mcp-integrations: "企查查MCP (Risk), ERP, AP system, TMS"
---

## MCP 配置要求

```bash
# ~/.claude/.mcp.json
{
  "mcpServers": {
    "qcc-risk": { "url": "https://agent.qcc.com/mcp/risk/stream" }
  }
}
```

---

## QCC 增强功能 — 中国供应商风险摘要

### 每周风险仪表板

每周简报中包含中国供应商：

```
中国供应商风险摘要 — 企查查增强版
================================================================
高优先级预警:
  [供应商]: [风险类型] — [日期]前需采取行动

新增风险信号（过去7天）:
  [数量] 供应商新增预警
  [数量] 行政处罚
  [数量] 经营异常

供应商健康评分:
  战略型:  [X]% 健康 / [X]% 有风险
  战术型:  [X]% 健康 / [X]% 有风险
================================================================
```

---

## 强制输出头

```
任务:          [例如：每周供应链简报 -- 2024-03-18周]
配置:          [已加载：supply-chain.local.md / 未配置]
数据来源:      [ERP / AP / TMS / 企查查MCP / 风险监控]
企查查风险:    [企查查增强 / 标准]
```
