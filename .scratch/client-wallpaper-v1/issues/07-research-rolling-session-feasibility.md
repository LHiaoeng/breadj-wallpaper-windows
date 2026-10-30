# 核实实际使用滚动续期与撤销的最小适配边界

Type: research
Labels: wayfinder:research
Mode: AFK
Parent: ../map.md
Blocked by: 03

## Question

以已确认的按设备六个日历月不活跃过期为约束，核实现有 M0 公共客户端候选能否通过最小适配实现：服务端确认 Windows 前台主动操作，后台刷新和离线操作不延期；活动与刷新凭据轮换、重放、退出、改密、重置及冻结并发时不恢复旧授权。重点核对固定 `desktop.refreshDeadline`、注册客户端 30 天校验、刷新令牌自身到期及 `client_refresh_token_history` 的摘要保留和清理。给出可复用部分、必须改动的边界、数据与性能风险、最小验证实验和失败时需回到地图决定的选项。

只读调查后端 `D:/IdeaProjects/light-shadow-wallpaper-admin` 及现有隔离测试，不写生产代码、不运行真实账号或迁移；当前 36 项测试不是滚动目标的证据。结论须引用源码与行号、区分事实和建议，并保存独立研究成果供首版验收票使用。

## Comments

前置决策：[确定 M0 登录与普通用户账号操作方案](03-decide-account-route.md)。
