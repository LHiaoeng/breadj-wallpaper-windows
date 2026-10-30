# 核实公开目录与 Windows 本地使用的可复用契约

Type: research
Labels: wayfinder:research
Mode: AFK
Status: resolved
Assignee: codex/catalog-research
Parent: ../map.md
Blocked by:

## Question

核对后端公开目录与 Windows 源码：真实分页、ID、可见性、媒体/海报字段能否支撑首版？收藏接口和关系是否已存在？现有下载、导入、本地身份关联及多屏设置的最小接入点在哪里？哪些缺口必须新增接口，哪些只需客户端适配？区分源码事实、部署未知项和方案建议。

调查路径：本仓库 src/Lively，以及后端 D:/IdeaProjects/light-shadow-wallpaper-admin 的壁纸目录与收藏相关源码。只读，不运行网络或媒体下载，不改播放器。提供文件/行号、最小契约缺口和需要在“确定目录、收藏与本地使用契约”中选择的选项，不写实现任务清单。

## Comments

### Resolution · 2026-10-30

## Answer

已完成只读一手源码调查。公开分页已有基础能力；未找到生产收藏接口，认证测试中的模拟路由不能当现有业务。Windows 可复用 HTTP 下载和媒体导入能力，远端直链不适用图库 ZIP 流程；来源与远端 ID 的持久映射、同 ID 内容变更及最小接口缺口交由后续决策票定案。

成果：[公开目录与本地使用调查](../../../research/client-wallpaper-v1/catalog-local-seams.md)。报告已随提交 `140b1ded` 纳入 `codex/client-wallpaper-v1`；按用户要求统一保存到该分支，原独立调查分支已清理。

未运行应用、数据库、媒体下载或测试；真实部署返回及媒体兼容性仍未知。
