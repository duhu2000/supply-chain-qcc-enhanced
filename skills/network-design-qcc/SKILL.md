---
name: network-design-qcc
description: >
  设计和比较供应链网络场景，整合中国供应商情报。
  适用于：网络设计、供应链网络、仓库选址、
  配送中心、DC选址、近岸外包、回岸外包、
  离岸外包、设施整合、网络优化、自制或外购。

  **企查查MCP增强版**：用中国供应商稳定性数据和区域风险评估
  丰富网络场景。

  不适用于：承运商绩效（使用logistics-brief-qcc）、供应商评估
  （使用vendor-assessment-qcc）、支出分析（使用spend-analysis-qcc）。
license: Apache-2.0
metadata:
  author: Panaversity (企查查MCP增强版)
  version: "2.0"
  plugin-commands: "/supply-network-design-qcc"
  mcp-integrations: "企查查MCP (Company/Risk), Network optimisation MCP, ERP"
---

## MCP 配置要求

```bash
# ~/.claude/.mcp.json
{
  "mcpServers": {
    "qcc-company": { "url": "https://agent.qcc.com/mcp/company/stream" },
    "qcc-risk": { "url": "https://agent.qcc.com/mcp/risk/stream" }
  }
}
```

---

## QCC 增强功能 — 供应商稳定性输入

### 网络设计风险因素

涉及中国供应商的场景：

1. **供应商集中度风险**
   - 地理集中度分析
   - 单一来源脆弱性
   - 关键供应商企查查稳定性评分

2. **区域风险评估**
   - 省级供应商密度
   - 基础设施风险指标
   - 政策/监管环境

### 企查查网络输入

```
供应商稳定性输入 — 企查查增强版
================================================================
关键中国供应商:
  [供应商]: [稳定性评分] — [风险等级]
  [位置]:   [省/市]

集中度风险:
  [X]% 支出在[区域] — [风险评估]

建议行动:
  [多元化 / 双源采购 / 接受风险]
================================================================
```

---

## 强制输出头

```
任务:          [例如：网络设计 -- EMEA配送审查]
配置:          [已加载：supply-chain.local.md / 未配置]
数据来源:      [ERP / TMS / 企查查MCP / Network optimisation MCP]
企查查输入:    [企查查增强 / 标准]
```
