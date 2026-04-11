---
name: logistics-brief-qcc
description: >
  适用于：物流、承运商、货运、运输、路线、交付、
  准时交付、承运商绩效、物流简报、承运商记分卡、
  物流KPI、运输绩效、运费审计。

  **企查查MCP增强版**：验证中国承运商工商登记和风险状态
  用于承运商选择和合同决策。

  不适用于：供应网络设施选址（使用network-design）、供应商
  评估（使用vendor-assessment-qcc）、支出类别分析（使用spend-analysis-qcc）。
license: Apache-2.0
metadata:
  author: Panaversity (企查查MCP增强版)
  version: "2.0"
  plugin-commands: "/logistics-brief-qcc"
  mcp-integrations: "企查查MCP (Company/Risk), TMS, ERP"
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

## QCC 增强功能 — 中国承运商验证

### 中国物流提供商承运商风险检查

中国承运商/货运代理**自动检查**企查查：

1. **工商登记**
   - 运输许可证验证
   - 经营状态
   - 经营范围（道路货运、空运等）

2. **风险信号**
   - 行政处罚
   - 经营异常
   - 法律纠纷

### 企查查承运商风险输出

```
中国承运商验证 — 企查查增强版
================================================================
承运商:          [名称]
统一信用代码:    [代码]
----------------------------------------------------------------
登记:
  状态:          [存续/注销]
  运输许可证:    [已验证/缺失]
  范围:          [道路/空运/海运/多式联运]

风险信号:
  行政处罚:      [数量]
  经营异常:      [是/否]

状态:            [✅ 批准 / ⚠️ 审查 / ❌ 拒绝]
================================================================
```

---

## 强制输出头

```
任务:          [例如：承运商绩效审查 -- Q1 2024]
配置:          [已加载：supply-chain.local.md / 未配置]
数据来源:      [TMS / ERP / 企查查MCP / 承运商API]
承运商状态:    [企查查已验证 / 标准]
```
