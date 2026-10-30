# Triage Labels

本地任务通过顶部的 `Status:` 字段记录分诊状态。

| 技能角色 | 本地状态值 | 含义 |
| --- | --- | --- |
| needs-triage | needs-triage | 等待维护者评估 |
| needs-info | needs-info | 等待补充信息 |
| ready-for-agent | ready-for-agent | 规格明确，可交给代理实施 |
| ready-for-human | ready-for-human | 需要人工实施 |
| wontfix | wontfix | 不予处理 |

技能要求应用某个分诊标签时，将 Status 更新为对应值。
Wayfinding 任务的 claimed/resolved 状态按 issue-tracker.md 执行。
