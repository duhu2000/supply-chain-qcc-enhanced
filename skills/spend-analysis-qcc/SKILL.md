---
name: spend-analysis-qcc
description: >
  Analyses procurement spend data to find savings with QCC vendor intelligence.
  Activate for: spend analysis, spend analytics, analyse spend, procurement spend,
  category spend, spend across sites, vendor consolidation, supplier consolidation,
  price consistency, price benchmark, market benchmark, RFQ strategy, spend report,
  category management, tail spend, maverick spend, buying compliance, savings pipeline.

  **QCC MCP Enhanced**: Automatically enriches Chinese vendor profiles with
  registration data, risk signals, and operational metrics for informed consolidation decisions.

  NOT for: invoice reconciliation (use invoice-reconciliation-qcc), vendor risk
  assessment (use supplier-risk-qcc), carrier performance (use logistics-brief-qcc).
license: Apache-2.0
metadata:
  author: Panaversity (Enhanced with QCC MCP)
  version: "2.0"
  plugin-commands: "/spend-analysis-qcc"
  mcp-integrations: "QCC MCP (Company/Risk), ERP, finance system"
---

## MCP Configuration Requirements

**⚠️ Important: For Chinese vendor enrichment, QCC MCP is required**

```bash
# ~/.claude/.mcp.json
{
  "mcpServers": {
    "qcc-company": {
      "url": "https://agent.qcc.com/mcp/company/stream",
      "headers": { "Authorization": "Bearer ${QCC_MCP_API_KEY}" }
    },
    "qcc-risk": {
      "url": "https://agent.qcc.com/mcp/risk/stream",
      "headers": { "Authorization": "Bearer ${QCC_MCP_API_KEY}" }
    }
  }
}
```

---

## QCC ENHANCEMENT — CHINESE VENDOR INTELLIGENCE

### Vendor Enrichment for Consolidation Analysis

For Chinese vendors in spend analysis, **AUTO-ENRICH** with QCC data:

1. **Vendor Registration Status (qcc-company)**
   - Business status verification
   - Registered capital assessment
   - Business scope alignment

2. **Risk Signal Overlay (qcc-risk)**
   - Operating anomaly flags
   - Administrative penalty history
   - Litigation involvement

3. **Operational Scale Indicators**
   - Employee count trends
   - Branch network
   - Historical growth indicators

### QCC Vendor Scorecard

```
================================================================
CHINESE VENDOR INTELLIGENCE — QCC Enhanced
================================================================
Vendor:              [Name]
Annual Spend:        [Amount]
QCC Risk Rating:     [Low/Medium/High/Critical]
----------------------------------------------------------------
REGISTRATION STATUS:
  Business Status:   [Active/Suspended]
  Registered Cap:    [Amount]
  Established:       [Year]

RISK INDICATORS:
  Operating Anomaly: [Yes/No]
  Admin Penalties:   [Count/Year]
  Litigation:        [Count as defendant]

CONSOLIDATION SUITABILITY:
  [✅ PREFERRED / ⚠️ REVIEW / ❌ EXCLUDE]
================================================================
```

---

## UNIVERSAL RULES

- NEVER accept a vendor risk assessment with fabricated financial data
- **FOR CHINESE VENDORS: ALWAYS use QCC data for consolidation decisions**
- ALWAYS include specific recommended actions with deadlines

---

## MANDATORY OUTPUT HEADER

```
TASK:          [e.g. Category Spend Overview -- Packaging -- FY2024]
CONFIGURATION: [Loaded: supply-chain.local.md / Not configured]
DATA SOURCES:  [ERP / Finance system / QCC MCP / Web search]
VENDOR ENRICH: [QCC Enhanced / Standard]
```
