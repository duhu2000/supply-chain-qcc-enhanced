---
name: network-design-qcc
description: >
  Designs and compares supply chain network scenarios with Chinese supplier
  intelligence. Activate for: network design, supply chain network, warehouse
  location, distribution centre, DC placement, nearshoring, reshoring,
  offshoring, facility consolidation, network optimisation, make vs buy.

  **QCC MCP Enhanced**: Enriches network scenarios with Chinese supplier
  stability data and regional risk assessment.

  NOT for: carrier performance (use logistics-brief-qcc), vendor assessment
  (use vendor-assessment-qcc), spend analysis (use spend-analysis-qcc).
license: Apache-2.0
metadata:
  author: Panaversity (Enhanced with QCC MCP)
  version: "2.0"
  plugin-commands: "/supply-network-design-qcc"
  mcp-integrations: "QCC MCP (Company/Risk), Network optimisation MCP, ERP"
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

## QCC ENHANCEMENT — SUPPLIER STABILITY INPUT

### Network Design Risk Factors

For scenarios involving Chinese suppliers:

1. **Supplier Concentration Risk**
   - Geographic concentration analysis
   - Single-source vulnerability
   - QCC stability scores for key suppliers

2. **Regional Risk Assessment**
   - Province-level supplier density
   - Infrastructure risk indicators
   - Policy/regulatory environment

### QCC Network Input

```
SUPPLIER STABILITY INPUT — QCC Enhanced
================================================================
Key Chinese Suppliers:
  [Supplier]: [Stability Score] — [Risk Level]
  [Location]: [Province/City]

Concentration Risk:
  [X]% of spend in [Region] — [Risk assessment]

Recommended Actions:
  [Diversification / Dual-sourcing / Accept risk]
================================================================
```

---

## MANDATORY OUTPUT HEADER

```
TASK:          [e.g. Network Design -- EMEA Distribution Review]
CONFIGURATION: [Loaded: supply-chain.local.md / Not configured]
DATA SOURCES:  [ERP / TMS / QCC MCP / Network optimisation MCP]
QCC INPUT:     [Enhanced / Standard]
```
