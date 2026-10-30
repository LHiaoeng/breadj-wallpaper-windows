# 核实账号安全候选与三端账号管理的真实接入边界

Type: research
Labels: wayfinder:research
Mode: AFK
Status: resolved
Assignee: codex/account-research
Parent: ../map.md
Blocked by:

## Question

核对现有 Java 后端和管理前端的一手源码：安全续期候选现已覆盖什么，M0 仍缺什么？普通用户与管理员入口、强制改密、开通/重置/冻结、浏览器会话及撤销分别应接在哪个现有扩展点？哪些已有能力可复用，哪些需要最小新增？明确候选默认关闭、测试边界、真实部署的未证实项，不重新设计身份平台。

调查路径：后端 D:/IdeaProjects/light-shadow-wallpaper-admin，管理端 D:/WebstormProjects/light-shadow-wallpaper-client-admin。对照首版 PRD 和 M0 进展；只读，不执行登录、数据库或迁移，不输出配置密钥。交付含文件与行号、事实/建议/未知项，以及供“确定 M0 登录与普通用户账号操作方案”决策使用的最小选项。

## Comments

### Resolution · 2026-10-30

## Answer

已完成只读一手源码调查。安全续期候选和资源边界可复用，完整 CUSTOMER 开通/重置、首次改密、受控交付、普通用户浏览器入口以及 Windows 恢复仍需补齐并联调。管理员通用新增用户流程不能直接作为普通账号开通流程。本票解决事实调查，不选定后续 HITL 方案，也不代表 M0 通过。

成果：[账号安全与管理端接入调查](../../../research/client-wallpaper-v1/account-boundaries.md)。报告已随提交 `140b1ded` 纳入 `codex/client-wallpaper-v1`；按用户要求统一保存到该分支，原独立调查分支已清理。

未运行应用、数据库或迁移；既有测试结果来自文档和源码，本次未重跑。
