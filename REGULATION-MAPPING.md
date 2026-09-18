# ACEF 法规映射明细（v0.1）

> 每个 ACEF 对象到具体法规条款的可追溯映射。合规报告的 `regulatory_mapping` 数组即此表的机器形态。

## 中国《智能体规范应用与创新发展实施意见》（网信办/发改委/工信部，2026-05-08）

| 条款 | 要求（摘要） | ACEF 对应 |
|---|---|---|
| §4 智能体注册平台 | 数字身份、能力声明、合规认证查询 | `subject.agent.registry_id`；`authorization-profile` 整体即"能力声明"的结构化形态 |
| §6 明确决策权限 | 操作不得超出用户授权范围；三分决策边界 | `authorization.decision_model` + `capabilities[].decision_class`；`behavior-event` 验证执行不越界 |
| §7 行为管控 | 规则内嵌、行为围栏、可验证可追溯（区块链） | `policy_source`（规则内嵌）；`outcome=blocked`（围栏生效证据）；`integrity` 哈希链 + `anchor`（可验证可追溯/存证） |
| §8 内生安全 | 权限管理、行为控制、安全评估体系 | `capabilities` 权限模型；`compliance-report` 评估产物 |
| §9 供应链安全 | 模型接入/API/扩展工具全周期管理 | `capabilities[model-access|network|tool-use]` 逐项声明 + 事件对账 |
| §11 分类分级 | 敏感领域备案/检测/召回；低风险"合规自测" | `compliance-report` 即自测报告；`regulatory_mapping` 供备案材料直接引用 |
| §12 合规服务体系 | 监测工具、第三方评测、认证结果互通互认 | 三个对象 = 互通互认的证据交换单元；`verifier.signature` 支撑第三方流水线 |

## EU AI Act（Regulation (EU) 2024/1689）

| 条款 | 要求 | ACEF 对应 |
|---|---|---|
| Art. 11 | 技术文档 | `authorization-profile`（系统与能力描述）+ 报告 |
| Art. 13 | 对部署方透明 | `summary`（双语受约束自然语言）+ `title` |
| Art. 72 | 部署后监测与严重事件上报 | `behavior-event` 持续流 + `violations[]`（0.2 加通报信封） |
| Art. 14 | 人工监督 | `requires_user_approval` + `escalated-to-user/user-approved` 事件闭环 |

## 美国 NIST（自愿性）

| 框架 | 条目 | ACEF 对应 |
|---|---|---|
| NIST AI RMF 1.0 | MEASURE 1.1/1.2 | 事件计数、围栏有效率 `fence_effectiveness` |
| NIST AI Agent Standards Initiative（2026） | consumer comparison 方向 | `summary` 双语摘要 + 逐条款状态，即"给非技术者对比"的最小单元 |
| ISO/IEC 42001 AIMS | 控制 A.8 运行控制 | `capabilities` 清单 + 事件证据 |

> 注：EU/NIST 条款号为 v0.1 概略映射，1.0 冻结前需经法律专业人士逐条核校（待办）。
