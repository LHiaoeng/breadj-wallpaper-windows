# 确定首版验收环境、证据与交付顺序

Type: grilling
Labels: wayfinder:grilling
Mode: HITL
Parent: ../map.md
Blocked by: 03, 04, 05, 07, 08

## Question

基于已定方案和原型，确定实施交接的完成条件：M0 与 M1 各需哪些真实 HTTP/持久层、Windows、多设备与多屏证据，在哪个隔离环境验收，缺失地址/账号/媒体样例由什么前置操作补齐，怎样判断可以启用候选及正式发布首版。

沿用 PRD 验收矩阵，明确测试替代范围与必须实机验证的部分；最终记录有依赖的交付顺序和待获取的外部条件。此票形成可交接路线，不执行发布，也不提前宣布验收通过。

## Comments

前置决策：[确定 M0 登录与普通用户账号操作方案](03-decide-account-route.md)、[用原型确定在线壁纸、收藏与账号异常交互](04-prototype-client-journeys.md)、[确定目录、收藏与本地使用契约](05-decide-data-contracts.md)。技术前提：[核实实际使用滚动续期与撤销的最小适配边界](07-research-rolling-session-feasibility.md)、[核实随机回调与浏览器重新认证的接入边界](08-research-browser-callback-session.md)。
