# 光影壁纸首版：账号、云端收藏与本地使用闭环

产品范围与验收基线：[光影壁纸 Windows 客户端首版 PRD](../../docs/光影壁纸Windows客户端首版PRD.md)。本文保留执行细节；其中 M0 安全续期“尚未定型”的描述是规格发布时状态，最新进展见 [M0 安全续期实现进展](../../docs/M0安全续期实现进展.md)。

首版交付前的关键方案与交互决策由 [光影壁纸首版交付决策地图](map.md) 组织；地图仅规划，不代表功能已经实现或验收。

Status: ready-for-agent

日期：2026-10-30  
范围：Windows 客户端、现有 Java 后端、必要的管理端操作。  
优先执行：补齐 M0 认证验证，再实施首版功能。产品基线已确认，测试边界已确认。

## Problem Statement

使用者需要在 Windows 客户端直接浏览并使用后台管理的壁纸，登录后保存自己的收藏，并在设备之间查看。现有客户端的 Lively 图库认证和 ZIP 安装协议与业务后端不同，不能靠替换服务器地址实现接入。

首版还缺少安全、可持续的普通用户登录。已有 CUSTOMER 用户类型和 OAuth2 基础设施，但默认认证不自动隔离后台权限，公共桌面客户端不能原样获得刷新令牌，临时密码也没有完整的强制改密及旧会话撤销流程。用户不应因这些技术限制而频繁输密码、误入管理接口，或在重置密码后仍留有有效旧登录。

目标是基于现有能力完成可正式运营的首版，账号由管理员开通，暂不提供自行注册；不一次性建设会员、营销和多端身份平台。

## Solution

游客可以浏览、预览、下载并设置公开壁纸，基础使用保持免费。需要保存个人云端收藏时才登录。首版复用既有 CUSTOMER 用户账号，由管理员受控开通及重置密码；每个账号使用独立临时密码，首次登录必须修改。

实际使用时登录有效期滚动延长，连续六个月没有实际使用才过期；后台自动刷新本身不算活跃。主动退出、冻结、改密或管理员重置可以使旧登录失效。收藏只有一个默认列表，联网操作、刷新同步；下架内容保留不可用占位，已经下载的文件继续保留和使用。

先以 M0 证明安全登录、续期、权限隔离和改密撤销，再交付浏览、收藏、下载与设置桌面的完整流程。签到与积分仅保留业务扩展边界，后续另行实施。

## User Stories

1. As a guest, I want to browse public wallpapers without an account, so that I can try the application immediately.
2. As a guest, I want to search wallpaper titles, so that I can find content I like.
3. As a guest, I want to filter images and videos, so that I can choose suitable wallpapers.
4. As a guest, I want to load additional results, so that I can explore the catalog without loading everything at once.
5. As a guest, I want clear loading, empty and failure states, so that I know whether to change my search or retry.
6. As a guest, I want to preview an image before downloading it, so that I can decide whether to use it.
7. As a guest, I want to start a video preview explicitly, so that browsing does not unexpectedly play media.
8. As a guest, I want previews to stop when I leave their details, so that playback does not continue unnoticed.
9. As a guest, I want to see available descriptions, sources and copyright information, so that I understand the content's origin.
10. As a guest, I want a broken cover to affect only that item, so that I can continue browsing.
11. As a guest, I want to download public wallpapers without logging in, so that account creation is not a barrier to basic use.
12. As a guest, I want to set a downloaded wallpaper on my desktop, so that browsing leads to a usable result.
13. As a user, I want my existing wallpaper to keep running while I browse or log in, so that exploration does not interrupt my desktop.
14. As a user, I want an administrator to open an ordinary account for me, so that I can use personal cloud features without self-registration.
15. As a user, I want a unique temporary password, so that my initial credentials are not shared with other users.
16. As a user, I want to replace my temporary password before accessing private features, so that I control my account's ongoing credentials.
17. As an account holder, I want secure desktop login without a bundled shared client secret, so that the application does not depend on an extractable global credential.
18. As an account holder, I want actual use to extend my login and six months without actual use to end it, so that background refresh cannot keep an unused login alive.
19. As an account holder, I want valid login credentials to survive an application restart securely, so that restarting does not require another login.
20. As an account holder, I want to cancel or retry a failed login safely, so that a failed callback cannot leave me partially logged in.
21. As an account holder, I want failed or expired authentication to be clearly explained, so that I can recover without losing public wallpaper access.
22. As an account holder, I want to log out of my current device, so that its saved credentials and private views are cleared.
23. As an account holder, I want password reset and account freezing to invalidate old sessions, so that an old login cannot continue to access private data.
24. As an account holder, I want to switch accounts without seeing the previous account's data, so that my collection remains private.
25. As an account holder, I want to favorite a wallpaper from its card or details, so that I can save it for later.
26. As a guest attempting to favorite an item, I want login to return me to that intended action, so that I do not have to find the item again.
27. As an account holder, I want to cancel login without creating a favorite, so that an unfinished action is not presented as complete.
28. As an account holder, I want repeated favorite requests to create only one relationship, so that repeated clicks or retries do not duplicate items.
29. As an account holder, I want to remove a favorite repeatedly without an error, so that retries leave the intended state intact.
30. As an account holder, I want one collection ordered by recent favorites, so that I can find saved items with minimal complexity.
31. As an account holder, I want to refresh my favorites on another device, so that I can use the same account collection across devices.
32. As an account holder, I want the application to tell me when a favorite operation requires a network connection, so that I do not mistake an offline action for a saved change.
33. As an account holder, I want unavailable favorites to remain as identifiable placeholders, so that content changes do not silently erase my collection.
34. As an account holder, I want to remove an unavailable favorite, so that deleted source content does not trap entries in my collection.
35. As an account holder, I want removing a favorite to preserve downloaded files, so that cloud organization does not delete local content.
36. As an account holder, I want deleting a local download to preserve my cloud favorite, so that local storage management does not change my collection.
37. As a user, I want download progress, cancellation and retry, so that large media transfers remain understandable and controllable.
38. As a user, I want failed or interrupted downloads to remain incomplete, so that unusable files are not shown as installed wallpapers.
39. As a user, I want repeated downloads to reuse an existing valid local item, so that the application does not create duplicate installations.
40. As a user, I want downloading to change my desktop only when I explicitly request it, so that saving a wallpaper does not unexpectedly apply it.
41. As a user, I want to use valid downloaded files offline, so that network or catalog availability does not remove my local wallpaper.
42. As a user with multiple monitors, I want wallpaper application to follow the existing monitor selection behavior, so that I can choose the intended screen.
43. As an administrator, I want a protected way to open CUSTOMER accounts without assigning management roles, so that ordinary users do not gain backend privileges.
44. As an administrator, I want to reset a forgotten password without viewing the old password, so that I can restore access safely.
45. As an administrator, I want account resets and freezes to be auditable, so that security-related changes can be traced.
46. As an administrator, I want existing management login and permissions to continue working, so that client integration does not disrupt operations.
47. As an ordinary account holder, I want to access only my own private data, so that another user's identifier cannot expose their collection.
48. As a user, I want readable controls in existing themes and window sizes, so that the new features remain accessible through the application's normal UI.

## Implementation Decisions

### 已确认的约束与边界

- 保持现有后端部署形态及分层，优先复用组件、服务与数据模型，不新增微服务或独立身份平台。
- 复用 sys_user 的 CUSTOMER 类型及既有 userId，收藏直接关联该身份，不新增独立 member/identity 账号表。不自动转换现有后台账号类型，不迁移未核实的其他终端账号。
- 普通用户不授后台角色。CUSTOMER 标记不是完整安全边界：管理员入口和管理接口需验证 SYSTEM 身份及原有权限，普通用户私有接口需验证 CUSTOMER 和数据归属。
- 受控开通不能直接使用强制创建 SYSTEM 的后台新增逻辑。服务端固定用户类型并排除管理员保留身份，不接受客户端指定角色、用户归属或特权状态。
- 首版不建设公开注册、短信/邮箱验证或自助找回。管理员开通/重置生成独立随机临时密码，哈希保存并受控交付，不记录明文。管理员只能重置，不能查询使用者真实密码。
- 首次改密前，服务端只放行必要的账号状态、改密和退出，拒绝私有业务；不能仅依赖前端弹窗。优先复用可用状态字段，确无则增加一个最小的持久化强制改密状态。
- 改密、重置、冻结及退出必须协调授权撤销、状态检查和并发刷新。不能只更新用户记录而继续接受旧身份快照；撤销后不得通过并发刷新恢复有效旧会话。

### 登录与 M0 技术关口

- 桌面按公共客户端约束设计，不内置共享 client secret，不把访问令牌直接延长至半年，不以频繁浏览器再授权代替已确认的登录体验。
- 系统浏览器、授权码和 PKCE 是既有候选登录路径；客户端须校验本次 state、一次性授权码、回调及 PKCE 绑定，取消/超时后清理监听和临时状态。
- 现有 SAS 0.4.2 公共客户端换码不签发刷新令牌，这已被组件和 Servlet 过滤器测试复现。安全续期具体实现尚未定型，不能把候选扩展写成既有能力。
- 下一执行步骤先证明一个复用现有授权服务的最小安全续期方案，覆盖用户/客户端绑定、有效期、轮换或等效抗重放措施以及撤销。不得只修改 Token 生成器就声称续期完成。
- 已登录用户在 Windows 前台主动浏览、搜索、收藏、下载、设置壁纸或操作账号时，服务端确认实际使用，并把该设备登录期限滚动延长至最近一次实际使用后的六个日历月；仅播放桌面壁纸、应用常驻、自动刷新或后台请求不计活跃。离线使用不延期，联网后仍有效的授权在下一次实际使用时延期，过期须重新登录；设备之间互不延期。退出、冻结、改密或凭据撤销可提前终止登录，重启恢复仍有效的授权。现有续期候选固定 30 天，须改动期限模型并重新验证。
- Windows 复用 DPAPI CurrentUser 加密，业务账号凭据与原 Lively 凭据分开存储。Token 只发往可信业务 API，不附加到媒体/CDN请求或日志中。
- 认证恢复采用单次在途协调；401 的重试有界，403 不触发无限登录循环。账号切换后丢弃旧账号响应、私有缓存和未完成动作。
- M0 以实际认证链、续期、隔离及改密撤销的结果为进入后续功能的门槛。若最小方案仍需明显改变架构，应记录具体失败证据及最小替代，不静默降低需求。

### 公开目录与媒体使用

- 公开目录复用现有壁纸查询与可见性规则，保留既有开放接口兼容。是否增加版本化公开目录入口，以真实契约缺口决定，不为改名复制业务逻辑。
- 默认每页 20 条，支持标题搜索和图片/视频筛选。切换查询时重置分页并忽略过期响应；请求成功后才推进页码，失败不跳页，按远端 ID 去重。
- 网络/业务失败与合法空结果分开显示；下一页失败保留已有内容。公开响应不混入个人收藏字段，不能通过共享缓存泄露用户数据。
- 列表展示封面和类型，详情展示可用描述、来源和版权。视频列表不自动播放，用户主动开始详情预览，离开时停止。
- 媒体地址与海报分别选择，沿用已核查的主/备用字段；候选去重、尝试次数有界。不猜测 URLBase 是媒体直链，不拼造高清地址。
- 远端只支持本次图片/视频媒体使用，不执行远端程序或网页。验证有效 HTTP(S) 地址及媒体内容，不把描述当可执行 HTML。

### 收藏契约与数据

- 首版一个默认集合，不创建多收藏夹、离线写队列或实时推送。进入页面、恢复前台及手动刷新时同步服务器状态。
- 私有收藏接口与现有匿名放行命名空间分开。拟沿用 /client/v1/me/favorites 契约，以 GET 分页、PUT 幂等收藏、DELETE 幂等取消；归属取认证主体，不能接受可替换的用户 ID。路由落地时核对既有冲突。
- 在线列表通过有数量上限的批量状态查询取得本人收藏状态，避免一张卡片一次请求。业务响应沿用现有统一返回与分页包装，OAuth 端点保留标准协议响应。
- 必要新业务数据为收藏关系，包含用户 ID、壁纸 ID、最小标题快照和收藏时间；用户与壁纸组合唯一，按用户、时间及稳定 ID 建分页索引。具体 DDL 对齐真实数据库版本及项目命名。
- 重复收藏不更新时间，取消后重新收藏形成新的收藏时间。操作进行中限制重复点击，成功后更新共享显示状态，失败保留原状态并允许重试。
- 下架/删除内容保留不可用占位和取消入口，不返回不可用资源的媒体直链；公开详情和收藏使用同一个可见性判定。
- 收藏与下载独立；内容停用和云端账号切换不主动删除本地公开媒体。首版不做本机下载文件的云账号隔离。

### 下载与本地库

- 复用现有下载、文件导入和桌面设置能力；单个图片/视频直链不能按原图库 ZIP 协议解压。
- 以服务标识和远端壁纸 ID 关联本地资源，避免与 Lively 原图库冲突。以实际有效文件判断下载状态，重启后恢复真实状态。
- 文件先下载到临时位置，校验后导入本地库；失败、取消、磁盘不足和导入失败不留下已安装假状态。未知长度显示不确定进度，不伪造百分比。
- 同一资源重复操作复用有效本地项或正在进行的任务，不重复创建安装记录。下载完成只有用户本次明确要求设置时才应用桌面。
- 多显示器沿用现有选择和布局行为；设置成功以桌面核心结果为准，不能以下载完成代替。

### 后续扩展边界

- 签到、积分、任务和权益不进入首版实现，不提前创建相应表或规则引擎。未来业务统一引用现有用户 ID。
- 后续签到由服务端按 Asia/Shanghai 确定业务日期，同一用户同日唯一；积分发放使用唯一业务键，签到事实、奖励流水和余额需保持事务一致。
- 积分用于装饰或额外功能，基础壁纸浏览、下载及设置免费。具体奖励、兑换、等级与权益另做规格，不作为本次隐藏交付项。

## Testing Decisions

- **已确认的主测试边界**：实际认证/业务 HTTP 接口及完整安全链。尽量在这一外部边界断言状态码、返回值、授权效果和持久化结果，不断言私有方法、类数量或内部调用顺序。
- **复用既有先例**：后端已有 JUnit、Mockito、实际 SAS Provider 与 Servlet 过滤器的 11 项认证兼容性测试；这些测试定位旧组件行为，不替代完整应用上下文和实际持久层验证。既有控制器契约测试可辅助检查映射，但权限是否成立要以真实请求结果判定。
- **M0 验收矩阵**：正确/缺失/错误 PKCE，授权码重放，错误 state 与回调绑定，用户取消，公共客户端无共享密钥续期，访问/刷新凭据到期，重启恢复，刷新重放，401 单次协调，403 无循环，退出、改密、冻结和重置的失效效果。
- **权限矩阵**：游客、CUSTOMER、SYSTEM 分别访问公开目录、本人数据、他人数据和管理接口；覆盖未单独写权限注解的管理路径。未登录应为 401，无权限为 403；现有后台权限及公开消费者保持兼容。
- **强制改密矩阵**：每账号临时密码独立，改密前通过直接 HTTP 请求也无法操作私有业务；正常改密后可访问，重置后旧密码及旧会话均无效；并发刷新与重置/冻结不能恢复被撤销会话。
- **收藏矩阵**：两设备并发收藏仅一条；重复取消成功；刷新同步；不可用内容占位且可取消；无媒体直链泄露；账号切换时晚到响应不能污染新账号。验证数据库唯一约束，不只模拟返回成功。
- **目录及媒体矩阵**：中文搜索与参数编码、分页重试不跳页、条件切换不回写旧数据、业务失败不假空、封面失败局部降级、视频主动播放及退出停止、主/备用资源失败有界。
- **Windows 必要联调边界**：浏览器与 loopback 回调、端口占用、取消/超时、DPAPI 凭据读写、重启与退出，以及图片/视频下载、取消、导入和多屏设置。现有本地库和播放器无需为本需求重写或扩充一套模拟框架。
- **下载故障矩阵**：断网、损坏媒体、磁盘不足、取消、导入失败、重复下载及重启，均不显示假完成；取消收藏不删文件，删除文件不取消收藏，已有公开媒体可离线使用。
- 续期测试通过可控时间/到期数据覆盖前台实际使用、后台自动刷新不延期、离线操作不延期、设备独立期限和连续六个日历月不活跃边界，不靠真实等待六个月；同时记录实际部署与重启验证范围，不能把 TTL 配置或内存测试描述为长周期生产验证。
- 使用隔离测试数据库、缓存和测试账号验证真实存储与撤销一致性。不得运行会写真实客户端配置的数据初始化测试来代替测试夹具，不记录 Token、密码或签名 URL。
- 代码修改按受影响模块执行格式、定向测试及构建；不因局部测试通过就宣布端到端通过。现有未提交内容不得回滚或混入提交，本次规格发布不执行 Git commit。

## Out of Scope

- 公开自助注册、自助密码找回、邮箱/短信供应商及第三方登录。
- 独立用户身份平台、微服务、事件总线、消息队列和营销规则引擎。
- 首版签到、积分账户、奖励发放、积分兑换、等级、会员、订单或支付。
- 多收藏夹、离线收藏队列、实时跨设备推送及收藏增量同步协议。
- 自动轮播、后台自动签到、断点续传承诺、远程 HTML/可执行程序壁纸及用户上传。
- 重写 Lively 播放核心、后台整体权限系统或无关业务；不承诺自动迁移其他终端身份。
- 本技能仅发布规格，不在本轮实施生产认证适配、数据库迁移或客户端功能。

## Further Notes

- 这是整个首版的执行规格；立即执行范围先限 M0。用户已于本次访谈结束时确认产品基线，并在规格发布前确认本规格的测试边界。
- M0 已有 11 项隔离测试通过，观测到公共客户端无刷新令牌、默认刷新信任旧用户快照和 authenticated-only 不隔离 CUSTOMER；这些是缺口证据，不是首版已实现的证明。
- ready-for-agent 表示执行范围足够明确，可以从 M0 方案验证开始；不表示安全续期已选型成功或完整 M0 已验收。账号续期的技术关口不得被下游收藏/UI任务绕过。
- 后端已有独立的 M0 调查记录与测试，不重新发布第二份冲突的产品规格；跨仓库实现以本规格及已接受 ADR 为产品约束，后端调查记录保留为事实证据。
- 关联依据：[首版方案](../../docs/客户端账号收藏与成长体系方案.md)、[M0 验证报告](../../docs/M0账号认证验证报告.md)、[术语表](../../GLOSSARY.md)、[公开壁纸与账号边界 ADR](../../docs/adr/0001-public-wallpapers-and-account-boundaries.md)、[CUSTOMER 账号开通 ADR](../../docs/adr/0002-customer-accounts-with-admin-provisioning.md)。
- 本次只发布规格，不拆分额外事项或扩展产品范围。后续任务应按 M0 → 首版完整闭环顺序创建，并保持各自依赖明确。
