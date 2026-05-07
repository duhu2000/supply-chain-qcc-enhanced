---
name: 供应商年度健康体检-supplier-annual-check-qcc
description: >
  供应商年度健康体检 SKILL · 企查查 MCP V2.0 增强版。
  供应商年度评审的标准化核查工具。对核心供应商进行全面年度体检，V2.0 新增真实财务 YoY 对比 + 双随机抽查 + 历史对比三层能力，一次性输出经营状态变化、资质证件到期情况、信用记录变更、财务指标退化预警的结构化报告。

  核心能力：
  - **V2.0 新能力**：`get_financial_data` 3 年财报 YoY 对比
  - **V2.0 新能力**：`get_random_check` 双随机抽查记录
  - 资质证件到期提醒（含续期风险）
  - 经营状态 YoY 变化（参保人数 / 招聘 / 招投标 / 荣誉）
  - 信用记录变更（YoY 新增失信 / 限高 / 行政处罚）
  - 法代 × 核心高管变动跟踪

  适用场景：采购供应商年度评审 / 续签决策 / 供应商分级调整 / 战略供应商健康度跟踪。

  使用方式：/supplier-annual-check 供应商名称 [--baseline 上期评审日] [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Supply Chain QCC (Enhanced with MCP V2.0)
  version: "2.0"
  plugin-commands: "/supplier-annual-check"
  mcp-integrations: "qcc-company, qcc-risk, qcc-operation, qcc-history, qcc-executive"
  industry: "Supply Chain - Vendor Management"
---

## 📖 QCC MCP 术语对照表（强制工具映射）

> **使用约定**：本表列出 SKILL 内业务简写与企查查 MCP 工具的精确映射。AI 执行本 SKILL 时遇到下表"业务简写"列的词汇，**必须调用对应"MCP 工具"列**，禁止使用 web search 或自由文本推测替代。完整规范见 [QCC-MCP-TERMINOLOGY.md](../../QCC-MCP-TERMINOLOGY.md)。

| 业务简写 | 规范全名 | 企查查 MCP 工具 |
| --- | --- | --- |
| 失信 | 失信被执行人 | `mcp__qcc-risk__get_dishonest_info` |
| 被执行 | 被执行人 / 判决债务人 | `mcp__qcc-risk__get_judgment_debtor_info` |
| 限高 | 限制高消费 | `mcp__qcc-risk__get_high_consumption_restriction` |
| 限出境 / 限境 | 限制出境 | `mcp__qcc-risk__get_exit_restriction` |
| 终本 | 终结本次执行案件 | `mcp__qcc-risk__get_terminated_cases` |
| 破产 / 重整 | 破产重整 | `mcp__qcc-risk__get_bankruptcy_reorganization` |
| 经营异常 | 经营异常 | `mcp__qcc-risk__get_business_exception` |
| 严重违法 | 严重违法失信 | `mcp__qcc-risk__get_serious_violation` |
| 行政处罚 / 重大处罚 | 行政处罚 | `mcp__qcc-risk__get_administrative_penalty` |
| 股权冻结 | 股权冻结 | `mcp__qcc-risk__get_equity_freeze` |
| 股权出质 | 股权出质 | `mcp__qcc-risk__get_equity_pledge_info` |
| 欠税 | 欠税公告 | `mcp__qcc-risk__get_tax_arrears_notice` |
| 税务异常 / 税务违法 | 税务异常 / 税收违法 | `mcp__qcc-risk__get_tax_abnormal` / `mcp__qcc-risk__get_tax_violation` |
| 受益所有人 / UBO | 受益所有人 | `mcp__qcc-company__get_beneficial_owners` |
| 实控人 / 实际控制人 | 实际控制人 | `mcp__qcc-company__get_actual_controller` |
| 主要人员 / 董监高 | 主要人员 | `mcp__qcc-company__get_key_personnel` |
| 抽查检查 / 双随机 | 双随机抽查 | `mcp__qcc-operation__get_random_check` |
| 吊销 | （登记状态字段判断）| 调 `mcp__qcc-company__get_company_registration_info` 取"登记状态" |
| 资不抵债 | （资产负债率字段判断）| 调 `mcp__qcc-company__get_financial_data` 判断负债率 > 100% |

---

# 供应商年度健康体检 · 企查查 MCP V2.0 增强版

## SKILL 定位

本 SKILL 服务于采购部门对战略供应商 / 核心供应商的年度定期评审场景。与"准入评估"的点式核查不同，年度体检的核心是"时间轴对比"——上期评审日 vs 本期评审日 YoY 变化。V2.0 最关键升级是 `get_financial_data` 让财务指标的 YoY 退化成为可量化的硬数据。

## MCP 依赖

- 必选：`qcc-company`（含 `get_financial_data`）/ `qcc-risk` / `qcc-history`
- 建议：`qcc-operation`（含 `get_random_check`）/ `qcc-executive`

## 工作流

### 维度一：基准日期声明
- 上期评审日期 / 本期评审日期 / 监控周期（12 个月 / 18 个月等）

### 维度二：**财务 YoY 对比**（V2.0 核心新能力）
`mcp__qcc-company__get_financial_data` —— 对比基准日和本期的营业收入 / 净利润 / 资产负债率 / 速动比率 / 经营现金流

**预警阈值**：
- 资产负债率同比 +15% 以上 → S 级预警
- 营收同比 -15% 以上 → S 级预警
- 净利润由正转负 → A 级预警
- 速动比率下降 > 0.3 → A 级预警

### 维度三：信用记录变更
- 本期新增失信 / 限高 / 被执行 / 股权冻结 → 立即上报
- 本期新增经营异常 / 税务违法 → A 级
- 本期新增行政处罚 → 按金额分级

### 维度四：经营指标 YoY
- 参保人数变化（大幅缩减 → 预警信号）
- 招投标活跃度 YoY
- 荣誉变化（新增 / 失效）
- **双随机抽查 YoY**（V2.0 新工具）

### 维度五：资质证件到期
- 本期 12 个月内到期的关键资质
- 需跟进续期的证书清单

### 维度六：人员变动
- 法代是否变更（最高优先级）
- 核心高管 YoY 离任数

## 预警分级与续签建议

| 预警级别 | 续签建议 |
|---------|---------|
| S 级 | 暂缓续签 + 业务部门会议决策 |
| A 级 | 附条件续签 + 加强监测 |
| B 级 | 正常续签 + 标准监测 |
| C 级 | 连续 2 期 C 级 → 考虑升级为战略供应商 |

## 输出模板

- 章节 1：年度体检 Decision Pack（预警级别 + 续签建议）
- 章节 2：基准日期声明
- 章节 3：**财务 YoY 退化分析**（V2.0 核心）
- 章节 4：信用记录变更（本期增量）
- 章节 5：经营 × 合规 YoY（含双随机抽查）
- 章节 6：资质到期 × 人员变动
- 章节 7：综合预警 × 续签建议

---

**SKILL 版本**：v2.0 | **所需 Server**：qcc-company / qcc-risk / qcc-history（必选）、qcc-operation / qcc-executive（建议）
