---
name: supply-chain-brief-qcc
description: >
  Activate for: supply chain brief, weekly supply chain report, CPO report,
  COO report, procurement dashboard, supply chain dashboard, weekly report,
  executive brief, supply chain KPIs, supply chain metrics, procurement
  performance, logistics performance summary, vendor performance summary.

  **QCC MCP Enhanced**: Includes Chinese vendor risk summary in weekly briefs
  for proactive risk management.

  NOT for: detailed vendor assessment (use vendor-assessment-qcc), detailed
  invoice reconciliation (use invoice-reconciliation-qcc).
license: Apache-2.0
metadata:
  author: Panaversity (Enhanced with QCC MCP)
  version: "2.0"
  plugin-commands: "/supply-chain-brief-qcc"
  mcp-integrations: "QCC MCP (Risk), ERP, AP system, TMS"
---

## MCP Configuration Requirements

```bash
# ~/.claude/.mcp.json
{
  "mcpServers": {
    "qcc-risk": { "url": "https://agent.qcc.com/mcp/risk/stream" }
  }
}
```

---

## QCC ENHANCEMENT — CHINESE VENDOR RISK SUMMARY

### Weekly Risk Dashboard

Include in weekly brief for Chinese vendors:

```
CHINESE VENDOR RISK SUMMARY — QCC Enhanced
================================================================
High Priority Alerts:
  [Vendor]: [Risk type] — Action required by [date]

New Risk Signals (Past 7 Days):
  [Count] vendors with new alerts
  [Count] administrative penalties
  [Count] operating anomalies

Vendor Health Score:
  Strategic:  [X]% healthy / [X]% at risk
  Tactical:   [X]% healthy / [X]% at risk
================================================================
```

---

## MANDATORY OUTPUT HEADER

```
TASK:          [e.g. Weekly Supply Chain Brief -- Week of 2024-03-18]
CONFIGURATION: [Loaded: supply-chain.local.md / Not configured]
DATA SOURCES:  [ERP / AP / TMS / QCC MCP / Risk monitoring]
QCC RISK:      [Enhanced / Standard]
```
