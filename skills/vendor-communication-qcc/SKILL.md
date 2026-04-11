---
name: vendor-communication-qcc
description: >
  适用于：供应商沟通、供应商通信、争议函、
  发票争议、纠正措施请求、价格争议、绩效通知、
  供应商警告、退出通知、合同通知、不续签通知。

  **企查查MCP增强版**：用中国企业风险数据丰富供应商通信
  用于知情的争议解决和升级决策。

  不适用于：发票处理（使用invoice-reconciliation-qcc）、供应商评估
  （使用vendor-assessment-qcc）、支出分析（使用spend-analysis-qcc）。
license: Apache-2.0
metadata:
  author: Panaversity (企查查MCP增强版)
  version: "2.0"
  plugin-commands: "/vendor-communicate-qcc"
  mcp-integrations: "企查查MCP (Company/Risk), Email MCP"
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

## QCC 增强功能 — 供应商风险背景

### 中国供应商风险感知通信

向中国供应商起草通信前**检查企查查**：

1. **供应商稳定性**
   - 经营状态（存续/注销）
   - 近期行政处罚
   - 涉诉情况

2. **通信语气调整**
   - 高风险供应商：正式、记录在案、法律审查
   - 稳定供应商：协作、解决方案导向

### 企查查背景输出

```
供应商风险背景 — 企查查增强版
================================================================
供应商:          [名称]
通信类型:        [争议/纠正措施请求/退出通知]
----------------------------------------------------------------
企查查风险档案:
  状态:          [存续/注销]
  风险等级:      [低/中/高]
  近期问题:      [数量]

通信建议:
  [标准 / 正式法律 / 上报高级管理层]
================================================================
```

---

## 强制输出头

```
任务:          [例如：供应商通信 -- 发票争议]
供应商层级:    [战略型 / 战术型 / 普通型 / 瓶颈型]
配置:          [已加载：supply-chain.local.md / 未配置]
企查查背景:    [企查查增强 / 标准]
```
