# ACEF — Agent Compliance Evidence Format

> 智能体合规证据格式（v0.1 草案 · 2026-09-18）
>
> **English abstract:** ACEF is an open interchange format for *compliance evidence* of AI agents:
> an **Authorization Profile** (what an agent is allowed to do, declared before execution),
> a **Behavior Event** stream (what the agent actually did, tamper-evident),
> and a **Compliance Report** (a derived, bilingual, human-readable attestation mapping
> observed behavior to regulatory provisions). ACEF does not compete with policy languages
> or enforcement engines — it standardizes the evidence layer nobody owns yet.

## 1. 为什么需要 ACEF

AI 智能体正在接管软件的开发与执行，但三个辖区同时指向同一件**尚不存在的东西**：

| 辖区 | 要求 | 出处 |
|---|---|---|
| 中国 | "智能体执行操作**不得超出用户授权范围**"、行为"**可验证、可追溯**"、"**合规自测**"、认证结果"**互通互认**" | 《智能体规范应用与创新发展实施意见》§6/§7/§11/§12（2026-05-08 三部委） |
| 欧盟 | 技术文档（Art 11）、对部署方的透明度（Art 13）、部署后监测与事件报告（Art 72） | EU AI Act 2024/1689 |
| 美国 | 自愿性安全评估以支持 "consumer comparison"（给非技术者看的对比） | NIST AI Agent Standards Initiative（2026-02 启动） |

共同产物 = **机器可验证的「授权范围声明 + 行为留痕 + 合规报告」交换格式**。
没有它：认证机构无法互认、买软件的人无法审阅、监管无法抽查。

## 2. 定位（重要：我们不做什么）

```
┌─────────────────────────────────────────────────┐
│  应用层：给非技术者看的合规报告 / 监管抽查包          │  ← ACEF 第 3 对象
├─────────────────────────────────────────────────┤
│  证据层：授权声明 + 行为事件的交换格式（ACEF）        │  ← 本规范 ★ 空位
├─────────────────────────────────────────────────┤
│  执法层：Cedar / Dogwood / OPA / AgentSpec / 沙箱  │  ← 已有大厂标准，不重造
└─────────────────────────────────────────────────┘
```

**Non-goals：**
- 不发明新的策略语言（直接引用/导入 Cedar、CEL、AgentSpec 表达式）
- 不替代运行时强制（enforcement 由现有引擎负责，ACEF 只消费其输出）
- 不做身份体系（对接 W3C Agent Identity Registry CG 的凭证工作）

## 3. 三个核心对象

| 对象 | 文件 | 回答的问题 | 谁读 |
|---|---|---|---|
| 授权声明 `authorization-profile` | `spec/authorization-profile.schema.json` | 这个智能体**被允许**做什么？ | 买主/监管/认证机构（签约前审） |
| 行为事件 `behavior-event`（JSONL 流） | `spec/behavior-event.schema.json` | 它**实际做了**什么？每次尝试的结果？ | 机器（事后审计、比对） |
| 履约报告 `compliance-report` | `spec/compliance-report.schema.json` | 实际行为与授权、与法规的**符合度**？ | 非技术决策者（签字页） |

设计原则：
1. **声明先于执行**——profile 在部署前签署，事件流只允许引用已声明的能力或记为越权；
2. **哈希链防篡改**——事件流自带 `prev_hash` 链，可选锚定到外部（中国 §7 提及区块链，格式留出 `anchor` 字段）；
3. **双语摘要**——报告的 `summary` 字段为受约束自然语言（zh-CN + en），这是"无编程基础者可审计"的落点；
4. **法规映射可机检**——每条符合性结论挂法规条款号与证据引用（`evidence_refs`）。

## 4. 快速上手

```bash
# 校验示例（需 pip install jsonschema）
py -m jsonschema --instance examples/edu-agent/authorization-profile.json spec/authorization-profile.schema.json
```

示例目录 `examples/edu-agent/` 是一个完整的垂直场景（教育智能体：课件生成 + 作业批改，
含一次被拦截的目录越权、一次升级人工确认），三对象互相引用，可直接当 demo 讲。

## 5. 版本与一致性用语

- 本版 `acef_version: "0.1"`，预采纳期，破坏性变更不受承诺约束；
- 规范用语借用 RFC 2119：MUST / SHOULD / MAY；
- 一致性级别：**Producer**（生成 ACEF 文件的工具）、**Consumer**（校验/阅读工具）。
  v0.1 只定义 Producer MUST；Consumer 行为 0.2 再约束。

## 6. 路线图

| 版本 | 内容 |
|---|---|
| 0.1（本版） | 三对象 schema + 教育示例 + 中欧美法规映射 |
| 0.2 | 签名信封（Sigstore/in-toto attestation 兼容）、Consumer 要求、Cedar/Dogwood 导入器规范 |
| 0.3 | 事件流锚定（anchor）与增量报告；多智能体派生（子 agent 继承声明） |
| 1.0 | 提交 OWASP GenAI 项目 / W3C CG 讨论，冻结 6 个月后发布 |

## 7. 许可、版权与商标

- **版权**：Copyright (c) 2026 The ACEF Authors。代码与文档按 **Apache-2.0** 授权（允许商业与闭源衍生，附 NOTICE 与 MARKS.md 约束）
- **贡献**：见 [CONTRIBUTING.md](CONTRIBUTING.md)——入站=出站（Apache-2.0）+ DCO 签署
- **商标**："ACEF" 名称受 [MARKS.md](MARKS.md) 使用政策约束（项目拟注册商标）。代码开放 ≠ 名称开放

<!-- SPDX-License-Identifier: Apache-2.0 -->
