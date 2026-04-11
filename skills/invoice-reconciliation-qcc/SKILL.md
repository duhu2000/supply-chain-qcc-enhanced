---
name: invoice-reconciliation-qcc
description: >
  Activate for: invoice, reconcile, reconciliation, three-way match, two-way
  match, invoice processing, AP, accounts payable, purchase order match,
  goods receipt match, invoice exception, price variance, quantity variance,
  duplicate invoice, invoice approval, invoice hold, invoice dispute, PO
  mismatch, over-invoiced, under-delivered, missing PO.

  **QCC MCP Enhanced**: Automatically validates Chinese vendor registration
  status, bank account details, and risk signals before payment authorization.

  NOT for: bank reconciliation (use banking plugin), vendor assessment
  (use vendor-assessment-qcc), spend category analysis (use spend-analysis-qcc).
license: Apache-2.0
metadata:
  author: Panaversity (Enhanced with QCC MCP)
  version: "2.0"
  plugin-commands: "/invoice-reconcile-qcc"
  mcp-integrations: "QCC MCP (Company/Risk), ERP, AP system"
---

## MCP Configuration Requirements

**⚠️ Important: For Chinese vendor validation, QCC MCP is required**

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

## QCC ENHANCEMENT — CHINESE VENDOR VALIDATION

### Phase 1: Vendor Identity Verification (Before Invoice Processing)

For Chinese vendors, **MANDATORY** QCC validation:

1. **Business Registration Check (qcc-company)**
   - Verify vendor name matches official registration
   - Check business status (active/liquidated/revoked)
   - Validate unified social credit code
   - Confirm registration address

2. **Bank Account Verification**
   - Cross-reference invoice bank details with QCC records
   - Flag discrepancies as potential fraud

3. **Risk Signal Check (qcc-risk)**
   - Operating anomaly status
   - Administrative penalties
   - Major legal disputes

### QCC Validation Output

```
================================================================
CHINESE VENDOR VALIDATION — QCC Enhanced
================================================================
Vendor Name:         [Name]
Invoice Bank:        [Account Details]
Query Date:          [YYYY-MM-DD]
----------------------------------------------------------------
REGISTRATION CHECK:
  Status:            [Active/Suspended/Liquidated]
  Credit Code:       [Verified/Not Found]
  Legal Rep:         [Name]

RISK SIGNALS:
  Operating Anomaly: [Yes/No]
  Admin Penalties:   [Count]
  Major Litigation:  [Count]

VALIDATION RESULT:
  [✅ CLEARED / ⚠️ ENHANCED REVIEW / ❌ REJECT]
================================================================
```

---

## UNIVERSAL RULES

- NEVER approve an invoice that has not been matched against a PO (above materiality threshold)
- NEVER process a duplicate invoice
- **FOR CHINESE VENDORS: NEVER process payment without QCC registration verification**
- **FOR CHINESE VENDORS: FLAG if invoice bank details differ from QCC records — fraud risk**
- ALWAYS include specific recommended actions with deadlines

---

## THREE-WAY MATCH ENFORCEMENT

Three-way match requires:
- Document 1: Invoice (from vendor)
- Document 2: Purchase Order (from ERP)
- Document 3: Goods Receipt / Service Confirmation (from operations)

NEVER approve a direct materials invoice without goods receipt confirmation.

---

## MANDATORY OUTPUT HEADER

```
TASK:          [e.g. Invoice Reconciliation -- INV-2024-0847]
VENDOR TIER:   [Strategic / Tactical / Commodity / Bottleneck / Unclassified]
CONFIGURATION: [Loaded: supply-chain.local.md / Not configured]
DATA SOURCES:  [ERP / AP system / QCC MCP / Manual input]
VENDOR STATUS: [QCC Validated / QCC Flagged / Non-Chinese Vendor]
```
