# 用原型确定在线壁纸、收藏与账号异常交互

Type: prototype
Labels: wayfinder:prototype
Mode: HITL
Parent: ../map.md
Blocked by: 03, 05

## Question

基于已定账号方案与目录/收藏/本地契约，给用户可操作的抛弃式原型，确定独立“在线壁纸”入口及其与本地库、收藏和账号入口的关系。覆盖游客收藏后登录并续接一次、首次改密、授权失效/切换账号、不可用收藏、下载/导入/失败重试与多屏设置的关键状态。

复用现有 WinUI 交互习惯，明确原图库入口如何呈现；原型不接真实凭据或持久层。只有用户看过并明确反馈后才能解决此票；产物保存研究分支并从本票引用。

## Comments

前置决策：[确定 M0 登录与普通用户账号操作方案](03-decide-account-route.md)、[确定目录、收藏与本地使用契约](05-decide-data-contracts.md)。
