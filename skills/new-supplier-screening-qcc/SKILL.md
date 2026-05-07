---
name: 新供应商快速筛选-new-supplier-screening-qcc
description: >
  新供应商快速筛选 SKILL · 企查查 MCP V2.0 增强版。
  招投标或采购寻源阶段对候选供应商的批量快速筛选工具。与"供应商准入评估"的深度核验不同，本 SKILL 聚焦"快速筛选 + 去伪存真"——一次扫描多家候选供应商，输出排序后的短名单。V2.0 新增双随机抽查作为合规筛选利器。

  核心能力：
  - 批量扫描候选供应商（每次可处理 5-20 家）
  - 9 项核心红线快筛（失信 / 限高 / 被执行 / 经营异常 / 破产 / 资不抵债 / 重大处罚 / 股权冻结 / 吊销）
  - **V2.0 新能力**：`get_random_check` 双随机抽查合规评分——经得起政府抽查 = 合规性强信号
  - 输出短名单（排除红线 + 按综合评分排序）

  适用场景：招标寻源阶段候选供应商排查 / 集中采购前的候选池筛选 / 新业务领域供应商批量扫描。

  使用方式：/new-supplier-screening 供应商 1 / 供应商 2 / ... [--top N 返回前 N 名] [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Supply Chain QCC Enhanced (MCP V2.0)
  version: "2.0"
  plugin-commands: "/new-supplier-screening"
  mcp-integrations: "qcc-company, qcc-risk, qcc-operation"
  industry: "Supply Chain - Procurement Sourcing"
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

# 新供应商快速筛选 · 企查查 MCP V2.0 增强版

## SKILL 定位

本 SKILL 是采购寻源的"筛子"——在进入正式"准入评估"或"IC Memo 级"深度尽调前，快速从候选池中排除"显然不合格"的主体。V2.0 `get_random_check` 是本场景的神器：**一家长期经得起政府双随机抽查、零违规记录的供应商，合规性上基本可信**——这比看资质证书更实在。

## MCP 依赖

- 必选：`qcc-company` / `qcc-risk`
- 强烈建议：`qcc-operation`（含 `get_random_check`）

## 工作流

### 维度一：9 项核心红线快筛

对每家候选供应商并行执行：

| 序号 | 工具 | 红线判定 |
|------|------|---------|
| 1 | `get_dishonest_info` | 当前失信 > 0 → 🔴 直接出局 |
| 2 | `get_high_consumption_restriction` | 当前限高 > 0 → 🔴 出局 |
| 3 | `get_judgment_debtor_info` | 当前被执行 > 0 → 🔴 出局 |
| 4 | `get_business_exception` | 当前经营异常 > 0 → 🔴 出局 |
| 5 | `get_bankruptcy_reorganization` | 已进入破产程序 → 🔴 出局 |
| 6 | `get_equity_freeze` | 股权冻结 > 0 → 🔴 出局 |
| 7 | `get_company_registration_info` | 登记状态吊销 / 注销 → 🔴 出局 |
| 8 | `get_tax_arrears_notice` | 欠税 > 0 → 🔴 出局 |
| 9 | `get_serious_violation` | 严重违法失信名单 → 🔴 出局 |

### 维度二：综合合规评分（未被红线排除的）

对通过红线的候选者计算综合评分：

| 指标 | 权重 | 评分逻辑 |
|------|------|---------|
| 纳税信用等级 | 25% | A=100 / B=70 / C=40 / D=0 |
| 海关信用等级 | 15% | 高级 AEO=100 / 一般认证=80 / 备案=50 |
| **双随机抽查记录**（V2.0）| 20% | 无违规=100 / 有违规已整改=70 / 有违规未整改=0 |
| 参保人数 | 10% | 100+=100 / 50-100=80 / 10-50=60 / <10=30 |
| 资质证书数量 | 10% | >10=100 / 5-10=80 / 1-5=50 / 0=0 |
| 招投标活跃度 | 10% | 高=100 / 中=70 / 低=30 |
| 荣誉信息 | 10% | 国家级=100 / 省级=70 / 市级=40 / 无=20 |

**综合评分 = Σ(指标得分 × 权重)**

### 维度三：短名单生成

1. 排除所有 🔴 红线触发者
2. 对剩余候选按综合评分降序排列
3. 返回 Top N（默认 Top 5）

## 输出模板

- 章节 1：筛选结果 Decision Pack（进入短名单的候选清单）
- 章节 2：筛选方法说明
- 章节 3：9 项红线排查结果（每家供应商一行）
- 章节 4：综合合规评分明细
- 章节 5：**短名单**（按排序） + 理由 + 下一步建议（IC Memo / 准入评估）

## 参数

- `--top N`：返回前 N 名，默认 5
- `--threshold <score>`：综合评分阈值，低于此值不入短名单
- `--format md|docx|pptx`：输出格式

---

**SKILL 版本**：v2.0 | **所需 Server**：qcc-company / qcc-risk（必选）、qcc-operation（强烈建议）
