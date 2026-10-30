# 首版账号安全与管理端接入调查

日期：2026-10-30。只读源码调查；没有运行登录、应用、数据库或迁移，没有读取配置值。事实与候选选项不等同产品决策。

## 已有能力

- 后端公共客户端续期候选按开关装配，启用前检查公共客户端注册、迁移字段、触发器及 InnoDB 表：[DesktopAuthenticationConfiguration.java:27](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/main/java/com/breadj/lightshadowwallpaper/admin/client/DesktopAuthenticationConfiguration.java:27)。这不证明目标部署已启用。
- 授权绑定账号 userId 与安全版本，账号状态变更使旧授权失效：[DesktopAuthorizationService.java:145](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/main/java/com/breadj/lightshadowwallpaper/admin/client/DesktopAuthorizationService.java:145)。迁移含强制改密状态、安全版本触发器和已消费刷新凭据摘要：[client-account-m0.sql:1](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/main/resources/db/client-account-m0.sql:1)。
- 资源过滤器区分 CUSTOMER 私有路径与 SYSTEM 路径，并为改密前账号预留状态、改密和退出路径：[ClientAccountBoundaryConfiguration.java:34](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/main/java/com/breadj/lightshadowwallpaper/admin/client/ClientAccountBoundaryConfiguration.java:34)。完整业务应用的路由覆盖仍需实测。
- 管理前端已有用户分页、新建、编辑、批量启用/锁定、角色与改密操作，可复用表格和弹窗组件：[SysUserPage.vue:25](D:/WebstormProjects/light-shadow-wallpaper-client-admin/src/views/system/user/SysUserPage.vue:25)、[用户 API:18](D:/WebstormProjects/light-shadow-wallpaper-client-admin/src/api/system/user/index.ts:18)。

## 最小缺口

- 通用用户表单允许管理员输入密码和选择角色，没有独立临时密码交付流程：[SysUserFormModal.vue:19](D:/WebstormProjects/light-shadow-wallpaper-client-admin/src/views/system/user/SysUserFormModal.vue:19)、[用户 DTO:77](D:/WebstormProjects/light-shadow-wallpaper-client-admin/src/api/system/user/types.ts:77)。ADR 已确认现有后台新增逻辑强制 SYSTEM，不可当 CUSTOMER 开通接口。
- 受控 CUSTOMER 开通/重置、首次改密业务端点和审计仍待补齐；资源过滤器预留路径不等于业务端点存在：[后端 M0 说明:96](D:/IdeaProjects/light-shadow-wallpaper-admin/docs/客户端账号M0安全续期.md:96)。冻结可复用现有账号状态，但需验证与撤销的一致性。
- 管理端当前密码/手机号 grant、localStorage 访问令牌及 401 清理流程不能充当桌面 PKCE 和长期恢复：[认证 API:14](D:/WebstormProjects/light-shadow-wallpaper-client-admin/src/api/auth/index.ts:14)、[user-store.ts:13](D:/WebstormProjects/light-shadow-wallpaper-client-admin/src/stores/user-store.ts:13)、[axios:54](D:/WebstormProjects/light-shadow-wallpaper-client-admin/src/utils/axios/index.ts:54)。
- Windows 的系统浏览器、loopback、state、取消/超时、DPAPI 和重启恢复，以及完整应用的用户/管理员登录入口隔离，仍属 M0 待验收范围：[M0 安全续期进展](../../docs/M0安全续期实现进展.md)。

## 待用户定案的选项

- 管理 UI：在系统用户页增加受限 CUSTOMER 操作，或增加独立“内测账号”页并复用组件；两者都调用专用 CUSTOMER 后端操作。
- 普通用户浏览器页面仅服务桌面授权，或另行提供普通用户站点。PRD 未要求后者，不应把管理员登录页视为普通用户站点。
- 临时密码具体受控交付渠道和一次展示规则；审计不留明文密码。
- 首次改密与旧授权撤销后的重新授权、浏览器退出和精确回调策略，需结合真实安全链与 Windows 能力选择。

## 证据边界与未知项

- 边界测试用实际资源过滤链和测试端点，不是完整业务应用：[ClientAccountBoundaryTest.java:39](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/test/java/com/breadj/lightshadowwallpaper/admin/client/ClientAccountBoundaryTest.java:39)。
- JDBC 安全链测试使用真实持久层，但密码重置是直接更新隔离数据库，不是管理员 HTTP 操作：[PublicClientRenewalJdbcTest.java:65](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/test/java/com/breadj/lightshadowwallpaper/admin/client/PublicClientRenewalJdbcTest.java:65)、[重置测试:215](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/test/java/com/breadj/lightshadowwallpaper/admin/client/PublicClientRenewalJdbcTest.java:215)。既有 36 项结果来自进展记录，本调查未重跑。
- 未证实目标部署的迁移、公共客户端注册、精确回调、候选开关、网关、表引擎和触发器、账号管理审计及交付渠道。不能以本地源码或隔离测试推断这些状态。

本报告仅解决事实调查；不代表续期候选已获启用或 M0 已通过。
