---
name: 供应商准入评估-vendor-assessment-qcc
description: >
  供应商准入评估 SKILL · 企查查 MCP V2.0 增强版。
  采购准入阶段的供应商深度尽调工具。输入供应商名称，AI 完成 9 维度风险评估，覆盖 34 类中国特有风险信号（司法执行 / 经营异常 / 税务违规 / 破产风险等），V2.0 新增双随机抽查 + 历史处罚两层维度，输出结构化准入报告。

  核心能力：
  - 基础工商核验 + 资质证书有效性
  - 司法风险 34 类扫描（当前层）
  - **V2.0 新能力**：历史行政处罚追溯（qcc-history）+ 双随机抽查合规评分（qcc-operation `get_random_check` 新工具）
  - 纳税信用 + 海关信用 + 政府监管评级
  - 法代 × 实控人个人风险快扫
  - 准入评级 A/B/C/D 输出

  适用场景：集中采购评审 / 国有企业供应商准入 / 甲方供应商库入库 / 招投标前资格预审。

  使用方式：/vendor-assess 供应商名称 [--category 类型] [--value 合同金额] [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Supply Chain QCC (Enhanced with MCP V2.0)
  version: "2.0"
  plugin-commands: "/vendor-assess"
  mcp-integrations: "qcc-company, qcc-risk, qcc-operation, qcc-history, qcc-executive"
  industry: "Supply Chain - Procurement / Vendor Management"
---

# 供应商准入评估 · 企查查 MCP V2.0 增强版

## SKILL 定位

本 SKILL 服务于采购评审委员会在供应商入库前的资格核查场景。V2.0 相对 V1.0 的升级聚焦两点：

- **`get_random_check`（双随机抽查）** 作为政府对企业的监管抽查合规评分，直接反映"企业在常规经营中是否经得起政府抽查"
- **qcc-history 历史行政处罚** 识别"曾经出险已修复"vs"连年处罚"两类供应商

## MCP 依赖

- 必选：`qcc-company` / `qcc-risk`
- 建议：`qcc-operation`（含 `get_random_check`）/ `qcc-history` / `qcc-executive`

## 工作流（9 维度）

### 维度一：工商基础核验
`get_company_registration_info` / `verify_company_accuracy` / `get_shareholder_info`

### 维度二：资质证书核验
`get_qualifications` / `get_administrative_license`

### 维度三：经营活跃度
`get_bidding_info` / `get_recruitment_info` / `get_honor_info`

### 维度四：政府信用评级
`get_credit_evaluation`（纳税信用）/ `get_import_export_credit`（海关信用）

### 维度五：**双随机抽查合规**（V2.0 新工具）
`mcp__qcc-operation__get_random_check` —— 返回企业被政府抽查的历史记录与结果。**无违规记录** = 经得起抽查的合规企业。

### 维度六：司法风险当前层
`get_dishonest_info` / `get_judgment_debtor_info` / `get_high_consumption_restriction` / `get_equity_freeze` / `get_tax_arrears_notice`

### 维度七：历史处罚追溯（V2.0 新能力）
`mcp__qcc-history__get_historical_admin_penalty` —— 识别 5 年内是否有已处罚但已结清的合规瑕疵，对"长期清洁"和"修复型供应商"差异化评估。

### 维度八：法代 × 实控人个人快扫（先扫后钻）
**先调 `mcp__qcc-executive__get_executive_risk_scan`（searchKey=企业 + personName=姓名，双锚）一次分诊法代/实控人 18 项个人风险维度 → 仅对 count>0 维度下钻对应 `get_executive_*` 原子**（如失信 `get_executive_dishonest`、限出境 `get_executive_exit_restriction`，对跨境供应商特别关键）；count=0 跳过，❌ 禁逐个散弹枪；多人则逐人各扫一次。

### 维度九：破产风险识别
`get_bankruptcy_reorganization` / `get_liquidation_info`

## 评级

| 评级 | 标准 | 准入建议 |
|------|------|---------|
| **A 级** | 工商真实 + 无司法风险 + 纳税 A 级 + 资质齐全 + 无双随机抽查违规 + 历史清洁 | **优先准入** |
| **B 级** | 无当前致命风险 + 历史已修复轻微事件 | **准入 + 加强监测** |
| **C 级** | 当前轻微风险 或 历史有已结清的中等处罚 | **附条件准入** + 担保要求 |
| **D 级** | 任一致命（失信 / 破产 / 资不抵债 / 实控人出险 / 吊销）| **拒绝准入** |

## 输出模板

- 章节 1：准入 Decision Pack（评级 + 准入建议）
- 章节 2：工商核验 × 资质 × 经营活跃度
- 章节 3：政府评级 × 双随机抽查（V2.0）
- 章节 4：司法风险 × 历史处罚（V2.0 双层）
- 章节 5：法代 × 实控人画像
- 章节 6：综合评级 × 准入决策

---

**SKILL 版本**：v2.0 | **适配 MCP 版本**：146 工具 / 6 Server 全量
**所需 Server**：qcc-company / qcc-risk（必选）、qcc-operation / qcc-history / qcc-executive（建议）
