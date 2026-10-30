# Repository Guidelines（仓库贡献指南）

## 项目结构与模块划分

本项目是 Windows 壁纸应用，解决方案入口为 `src/Lively/Lively.sln`：

- `Lively/`：WPF 核心、桌面集成与插件管理。
- `Lively.UI.WinUI/`：WinUI 界面、XAML 视图、资源与本地化字符串；`Lively.UI.Shared/` 存放共享视图模型和辅助代码。
- `Lively.Common/`、`Lively.Common.Services/`、`Lively.Models/`：公共工具、服务与数据契约。
- `Lively.Grpc.*`、`Lively.Player.*`、`Lively.Utility.*`：进程间通信、播放后端与辅助程序。

安装脚本位于 `src/installer/`，宣传素材位于 `resources/`，壁纸属性 Schema 位于 `schemas/`。仓库目前未包含自动化测试项目。

## 构建与本地开发

在 Windows 上安装 Visual Studio 的 .NET 桌面与 WinUI 开发工具、.NET 8/9 SDK、项目所需的 Windows SDK，以及旧项目依赖的 .NET Framework 4.7.2 目标包。通过 Visual Studio Developer PowerShell，在仓库根目录执行：

```powershell
dotnet --list-sdks
msbuild src/Lively/Lively.sln /restore /p:RestorePackagesConfig=true /p:Configuration=Debug /p:Platform=x64
```

第一条命令检查已安装的 SDK；第二条还原依赖并构建解决方案。构建发布版本时，将 `Debug` 改为 `Release`。

本地调试时，打开解决方案，选择 **Debug / x64**，将 **Lively** 设为启动项目，按 **F5**。先将完整 WinUI 构建输出放入 `src/Lively/Lively/Plugins/UI/`，将 mpv 运行文件放入 `src/Lively/Lively/Plugins/Mpv/`，再重新构建核心项目。已验证的启动步骤与插件缺失排查见 `项目启动与问题排查.md`。

## 编码风格与命名规范

遵循相邻 C# 与 XAML 文件的风格：使用四个空格缩进，C# 大括号单独成行。类型、方法与属性使用 PascalCase，参数与局部变量使用 camelCase，接口以 `I` 开头。沿用现有字段命名与模块边界。仓库未统一配置格式化工具、静态检查工具或 `.editorconfig`；提交时避免无关的格式调整。

## 测试要求

目前未配置测试框架或覆盖率门槛。修改后应构建受影响项目，并按变更范围手动验证壁纸播放、暂停与恢复、屏幕保护或多显示器布局。在 PR 中记录复现步骤与验证结果。新增自动化测试时，应说明所用框架、测试命名规范与具体执行命令。

## 提交与 PR 规范

Git 历史采用简短、描述性的提交标题，例如 `Fix Mpv slider serialization culture`；请沿用这一风格，每次提交聚焦一个主题。PR 应说明解决的问题与行为变化，关联相关 Issue，列出验证结果和运行依赖；界面变更应附截图。安全漏洞请按 `SECURITY.md` 中的方式报告。

## Agent skills

### Subagents

- 主 agent 的模型和推理强度由用户自由选择，项目只固定子 agent 默认值。
- 文案、简单 XAML 和局部小改动由主 agent 完成并自查；调用链不明确时按需委派 `explorer`。
- 一般业务逻辑和范围明确的修复按需委派 `reviewer`；涉及账号安全、文件删除、进程通信、并发或播放生命周期时，直接委派 `risk_reviewer`，避免两种审查重复执行。用户或技能明确要求的委派按其要求执行。
- 主 agent 统一修改文件、构建和验收；子 agent 只读，默认不递归委派。仅并行处理相互独立的任务，避免重复探索和构建。
- 委派时提供目标、相关路径、必要约束和交付格式；支持独立上下文时仅传递必要信息。收齐所需结果后由主 agent 整合，子 agent 只返回证据摘要。

### Issue tracker

规格和任务使用本地 Markdown；创建、读取或更新任务前，阅读 `docs/agents/issue-tracker.md`。

### Triage labels

使用五个默认分诊标签；分诊任务前，阅读 `docs/agents/triage-labels.md`。

### Domain docs

采用 single-context 布局；探索代码前，阅读 `docs/agents/domain.md`。
