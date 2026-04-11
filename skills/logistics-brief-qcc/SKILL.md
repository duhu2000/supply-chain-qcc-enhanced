---
name: logistics-brief-qcc
description: >
  Activate for: logistics, carrier, freight, shipping, route, delivery,
  on-time delivery, OTD carrier, logistics performance, carrier review,
  freight cost, cost per kg, lane analysis, route optimisation, logistics brief,
  carrier scorecard, logistics KPI, shipping performance, freight audit.

  **QCC MCP Enhanced**: Validates Chinese carrier registration and risk status
  for carrier selection and contract decisions.

  NOT for: supply network facility placement (use network-design), vendor
  assessment (use vendor-assessment-qcc), spend category analysis (use spend-analysis-qcc).
license: Apache-2.0
metadata:
  author: Panaversity (Enhanced with QCC MCP)
  version: "2.0"
  plugin-commands: "/logistics-brief-qcc"
  mcp-integrations: "QCC MCP (Company/Risk), TMS, ERP"
---

## MCP Configuration Requirements

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

## QCC ENHANCEMENT — CHINESE CARRIER VALIDATION

### Carrier Risk Check for Chinese Logistics Providers

For Chinese carriers/freight forwarders, **AUTO-CHECK** QCC:

1. **Business Registration**
   - Transport license verification
   - Operating status
   - Business scope (road freight, air cargo, etc.)

2. **Risk Signals**
   - Administrative penalties
   - Operating anomalies
   - Legal disputes

### QCC Carrier Risk Output

```
CHINESE CARRIER VALIDATION — QCC Enhanced
================================================================
Carrier:             [Name]
Unified Credit Code: [Code]
----------------------------------------------------------------
REGISTRATION:
  Status:            [Active/Suspended]
  Transport License: [Verified/Missing]
  Scope:             [Road/Air/Sea/Multimodal]

RISK SIGNALS:
  Admin Penalties:   [Count]
  Operating Anomaly: [Yes/No]

STATUS:              [✅ APPROVED / ⚠️ REVIEW / ❌ REJECT]
================================================================
```

---

## MANDATORY OUTPUT HEADER

```
TASK:          [e.g. Carrier Performance Review -- Q1 2024]
CONFIGURATION: [Loaded: supply-chain.local.md / Not configured]
DATA SOURCES:  [TMS / ERP / QCC MCP / Carrier APIs]
CARRIER STATUS: [QCC Validated / Standard]
```
