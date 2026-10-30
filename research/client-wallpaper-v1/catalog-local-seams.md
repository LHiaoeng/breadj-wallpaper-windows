# 首版公开目录与 Windows 本地使用调查

日期：2026-10-30。只读一手源码调查；没有运行应用、数据库、测试或网络请求。方案选项留待决策票与用户定案。

## 公开目录

- 已有 `GET /rest/wallpaper/page`，返回 `R<PageResult<WallpaperRestVO>>`，调用公开分页查询：[WallpaperRestController.java:27](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/main/java/com/breadj/lightshadowwallpaper/admin/openapi/WallpaperRestController.java:27)。本次未证实依赖中 PageParam 的默认值和页码起点。
- 查询限制启用、非空主 url，支持标题/类型筛选并按 ID、上架时间排序：[WallpaperMapper.java:64](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/main/java/com/breadj/lightshadowwallpaper/admin/wallpaper/mapper/WallpaperMapper.java:64)。只有备用资源的数据可能被排除，需确认是否属于实际数据缺口。
- DTO 已含 Long ID、图片/视频类型、标题、媒体/海报及其备用地址、描述、来源和版权：[WallpaperRestVO.java:19](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/main/java/com/breadj/lightshadowwallpaper/admin/wallpaper/model/vo/WallpaperRestVO.java:19)、[WallpaperQO.java:24](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/main/java/com/breadj/lightshadowwallpaper/admin/wallpaper/model/qo/WallpaperQO.java:24)。字段存在不证明媒体可访问，也不能据字段名断定 urlBase 可直接下载。

## 收藏

后端生产源码检索未找到收藏实现。`/client/v1/me/favorites` 出现在认证测试的模拟端点中，不能视为现成业务接口：[PublicClientRenewalJdbcTest.java:360](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/test/java/com/breadj/lightshadowwallpaper/admin/client/PublicClientRenewalJdbcTest.java:360)、[模拟端点:721](D:/IdeaProjects/light-shadow-wallpaper-admin/admin/src/test/java/com/breadj/lightshadowwallpaper/admin/client/PublicClientRenewalJdbcTest.java:721)。真实私有收藏接口和关系持久化仍需新增，须落实 PRD 已确认的归属、幂等、唯一约束和不可用占位。

## Windows 接入点

- 旧图库订阅下载 Lively ZIP；`AddWallpaperFileGallery` 拒绝非 Lively ZIP。不能拿它安装 JPG/MP4 直链：[LibraryViewModel.cs:444](D:/breadj-wallpaper-windows/src/Lively/Lively.UI.Shared/ViewModels/LibraryViewModel.cs:444)、[ZIP 校验:521](D:/breadj-wallpaper-windows/src/Lively/Lively.UI.Shared/ViewModels/LibraryViewModel.cs:521)。
- 本地媒体能力可从 `AddWallpaperFile(path, autoSetWallpaper)` 和批量媒体导入路径复用；后者使用 CreateMediaWallpaperPackageAsync 生成媒体包和缩略图。是否经交互式创建及如何确定导入完成，需在最终接入方案核对：[LibraryViewModel.cs:556](D:/breadj-wallpaper-windows/src/Lively/Lively.UI.Shared/ViewModels/LibraryViewModel.cs:556)、[批量导入:628](D:/breadj-wallpaper-windows/src/Lively/Lively.UI.Shared/ViewModels/LibraryViewModel.cs:628)、[WallpaperLibraryFactory.cs:145](D:/breadj-wallpaper-windows/src/Lively/Lively.Common/Factories/WallpaperLibraryFactory.cs:145)。
- HttpDownloadService 支持进度、取消和失败清理；媒体有效性和临时文件导入由调用者负责：[HttpDownloadService.cs:19](D:/breadj-wallpaper-windows/src/Lively/Lively.Common.Services/HttpDownloadService.cs:19)、[失败路径:67](D:/breadj-wallpaper-windows/src/Lively/Lively.Common.Services/HttpDownloadService.cs:67)。
- 既有 `LivelyInfoModel.Id` 为字符串图库 ID，没有独立来源字段；旧 Gallery 会写入该值。需决定采用有来源的命名空间还是最小独立来源元数据，并核对 ID 使用者：[LivelyInfoModel.cs:19](D:/breadj-wallpaper-windows/src/Lively/Lively.Models/LivelyInfoModel.cs:19)、[LibraryViewModel.cs:521](D:/breadj-wallpaper-windows/src/Lively/Lively.UI.Shared/ViewModels/LibraryViewModel.cs:521)。
- 本地库已有多屏选择与 SetWallpaper 路径，可复用用户交互：[LibraryViewModel.cs:148](D:/breadj-wallpaper-windows/src/Lively/Lively.UI.Shared/ViewModels/LibraryViewModel.cs:148)。实际图片/视频和多屏成功仍须实机验收。

## 待定契约与部署事实

- 目录优先沿用现有公开分页；若真实样例证明缺少详情/可用性能力，再讨论最小新增入口，不为路由改名复制逻辑。
- 收藏沿用已确认行为，决策票需明确最终路由、DTO、稳定分页与不可用项信息。
- 本地需决定来源与远端 ID 的持久映射、媒体导入完成判定，以及同 ID 内容变更时对已有有效文件的处理。
- API 根地址、网关前缀、匿名读取、CDN/防盗链、实际成功码和分页样例尚未证实。本地字段和测试桩不能代替部署契约。

调查不改变游客免费使用、CUSTOMER 复用及收藏与下载独立的 ADR 约束；没有锁定技术实现或宣布任何验收通过。
