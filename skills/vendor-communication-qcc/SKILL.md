---
name: vendor-communication-qcc
description: >
  Activate for: vendor communication, supplier communication, dispute letter,
  invoice dispute, corrective action request, CAR, price dispute, performance
  notice, vendor warning, exit notice, contract notice, non-renewal notice.

  **QCC MCP Enhanced**: Enriches vendor communications with Chinese enterprise
  risk data for informed dispute resolution and escalation decisions.

  NOT for: invoice processing (use invoice-reconciliation-qcc), vendor assessment
  (use vendor-assessment-qcc), spend analytics (use spend-analysis-qcc).
license: Apache-2.0
metadata:
  author: Panaversity (Enhanced with QCC MCP)
  version: "2.0"
  plugin-commands: "/vendor-communicate-qcc"
  mcp-integrations: "QCC MCP (Company/Risk), Email MCP"
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

## QCC ENHANCEMENT — VENDOR RISK CONTEXT

### Risk-Aware Communication for Chinese Vendors

Before drafting communications to Chinese vendors, **CHECK QCC** for:

1. **Vendor Stability**
   - Operating status (active/suspended)
   - Recent administrative penalties
   - Litigation involvement

2. **Communication Tone Adjustment**
   - High-risk vendor: Formal, documented, legal-reviewed
   - Stable vendor: Collaborative, solution-focused

### QCC Context Output

```
VENDOR RISK CONTEXT — QCC Enhanced
================================================================
Vendor:              [Name]
Communication Type:  [Dispute/CAR/Exit Notice]
----------------------------------------------------------------
QCC RISK PROFILE:
  Status:            [Active/Suspended]
  Risk Level:        [Low/Medium/High]
  Recent Issues:     [Count]

COMMUNICATION RECOMMENDATION:
  [Standard / Formal-Legal / Escalate to Senior Mgmt]
================================================================
```

---

## MANDATORY OUTPUT HEADER

```
TASK:          [e.g. Vendor Communication -- Invoice Dispute]
VENDOR TIER:   [Strategic / Tactical / Commodity / Bottleneck]
CONFIGURATION: [Loaded: supply-chain.local.md / Not configured]
QCC CONTEXT:   [Enhanced / Standard]
```
