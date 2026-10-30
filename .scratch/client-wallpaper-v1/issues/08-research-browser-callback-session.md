# 核实随机回调与浏览器重新认证的接入边界

Type: research
Labels: wayfinder:research
Mode: AFK
Parent: ../map.md
Blocked by: 03

## Question

核实本项目实际 Ballcat 1.4.2 / SAS 0.4.2 与当前部署形态下，公共 Windows 客户端能否使用随机可用的 `127.0.0.1` loopback 端口，并在退出本机后即使浏览器保留 Cookie 也要求重新确认身份。核对现有授权服务的回调匹配、公共客户端注册、系统浏览器登录页、浏览器会话及退出扩展点；提出保持管理端登录兼容的最小适配路线与必须验证的反例。首次改密撤销临时授权后重新取得正常授权也纳入接入链核查。

依据 [RFC 8252 §7.3](https://www.rfc-editor.org/rfc/rfc8252.html#section-7.3) 与实际项目版本，不按上游 2.x 推断 1.x 行为。只读调查后端和管理端源码，不执行真实登录、改配置或迁移；结论引用源码与行号，区分已验证事实、建议和目标部署未知项，保存独立研究成果供首版验收票使用。

## Comments

前置决策：[确定 M0 登录与普通用户账号操作方案](03-decide-account-route.md)。
