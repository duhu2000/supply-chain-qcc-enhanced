---
name: supply-chain-risk-qcc
description: >
  供应链风险扫描 SKILL · 企查查 MCP V2.0 增强版。
  供应链韧性管理的持续监控工具。对核心供应商批量实时扫描高危风险信号（失信被执行 / 破产重整 / 司法拍卖 / 法代个人失信 / 实控人出险），一旦出现立即触发预警。V2.0 新增"关键人员批量扫描"—— 通过 qcc-executive 对供应链上所有核心法代做个人画像快扫，识别"企业清洁但法代出险"的隐性风险。

  核心能力：
  - 批量供应商持续监控（50-200 家供应商一次扫描）
  - 11 项高危预警信号（失信 / 限高 / 被执行 / 股权冻结 / 破产 / 吊销 / 司法拍卖 / 经营异常 / 资不抵债等）
  - **V2.0 新能力**：关键人员批量扫描 —— 对供应链上 N 家供应商的 N 位法代同时做个人红线快扫
  - 预警级别 S/A/B + 推荐 Action
  - 供应中断应急响应链路

  适用场景：采购与供应链团队贷后持续监控 / 核心供应商每日健康度巡检 / 供应链风险管理驾驶舱 / 供应中断应急预案触发。

  使用方式：/supply-risk 供应商列表 [--frequency daily|weekly|monthly] [--alert-only 仅输出预警] [--format md|docx|pptx]

license: Apache-2.0
metadata:
  author: Supply Chain QCC Enhanced (MCP V2.0)
  version: "2.0"
  plugin-commands: "/supply-risk"
  mcp-integrations: "qcc-company, qcc-risk, qcc-executive, qcc-history"
  industry: "Supply Chain - Risk Monitoring"
---

# 供应链风险扫描 · 企查查 MCP V2.0 增强版

## SKILL 定位

本 SKILL 与"供应商年度体检"的差异在于"粒度 + 频次"——年度体检是深度评审（每年 1 次），本 SKILL 是浅层但批量的持续监控（日级 / 周级 / 月级）。适合供应链风险管理驾驶舱的自动化扫描。V2.0 最关键升级是 qcc-executive 允许对供应链上所有核心法代 **批量做个人红线快扫**，识别"企业当前清洁但法代已经出险（如限出境 / 失信）"的隐性风险——这常常是企业级风险的 3-6 个月前信号。

## MCP 依赖

- 必选：`qcc-company` / `qcc-risk`
- 强烈建议：`qcc-executive`（关键人员批量扫描）
- 建议：`qcc-history`（识别"修复型"供应商）

## 工作流

### 维度一：批量供应商扫描（11 项高危信号）

对列表中每家供应商并行调用：

| 信号 | 工具 | 预警级别 |
|------|------|---------|
| 1. 新增失信 | `get_dishonest_info` | S |
| 2. 新增限高 | `get_high_consumption_restriction` | S |
| 3. 新增被执行 | `get_judgment_debtor_info` | S |
| 4. 新增股权冻结 | `get_equity_freeze` | S |
| 5. 破产重整 | `get_bankruptcy_reorganization` | S（极端）|
| 6. 清算 | `get_liquidation_info` | S（极端）|
| 7. 吊销 / 注销 | `get_company_registration_info` 状态字段 | S |
| 8. 司法拍卖 | `get_judicial_auction` | A |
| 9. 新增经营异常 | `get_business_exception` | A |
| 10. 新增严重违法 | `get_serious_violation` | A |
| 11. 税务非正常户 | `get_tax_abnormal` | A |

### 维度二：**关键人员批量扫描**（V2.0 核心新能力）

对每家供应商获取法代姓名（`get_company_registration_info`），然后批量调用：

- `mcp__qcc-executive__get_executive_dishonest` —— 法代个人失信
- `mcp__qcc-executive__get_executive_exit_restriction` —— 法代限制出境（跨境业务强预警）
- `mcp__qcc-executive__get_executive_high_consumption_ban` —— 法代限高

**任一命中 → S 级预警**（企业当前清洁 + 法代出险 = 企业在 3-6 个月内极可能爆雷的先行信号）。

### 维度三：历史轨迹对比（可选 V2.0）

- `mcp__qcc-history__get_historical_dishonest` —— 识别"修复型供应商"vs"连年失信型"
- 对连年失信型标注"系统性风险"，提升监控频率

### 维度四：预警聚合与分发

- S 级预警 → **立即推送至采购经理 + 供应链风控 + 业务负责人**
- A 级预警 → T+3 内邮件汇总
- B 级 → 周报 / 月报汇总

## 预警与应急响应

| 级别 | 触发条件 | 应急动作 |
|------|---------|---------|
| **S 级** | 任一核心红线命中 或 法代个人出险 | 24h 内启动备供切换预案 + 暂停新增订单 + 应收账款回收评估 |
| **A 级** | 轻度负面信号（经营异常 / 税务非正常 / 司法拍卖）| T+3 内预警供应链风控 + 加强监测 |
| **B 级** | 无新增负面 但 财务或经营指标 YoY 下降 | 纳入周报 + 下次年度体检重点关注 |

## 输出模板

- 章节 1：本期扫描结果 Decision Pack（S/A/B 级预警数量 + 关键发现）
- 章节 2：扫描范围说明（供应商数 + 扫描维度）
- 章节 3：S 级预警清单（每家一行 + 触发信号 + 推荐 Action）
- 章节 4：A 级预警清单
- 章节 5：**V2.0 关键人员批量扫描结果**
- 章节 6：历史趋势（修复型 vs 连年失信型分布）
- 章节 7：应急响应建议清单

## 参数

- `--frequency <daily|weekly|monthly>`：扫描频次
- `--alert-only <true|false>`：是否只输出预警（默认 false，输出全量）
- `--format md|docx|pptx`：输出格式

---

**SKILL 版本**：v2.0 | **所需 Server**：qcc-company / qcc-risk（必选）、qcc-executive（强烈建议）、qcc-history（建议）
