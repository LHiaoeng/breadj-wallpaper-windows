# Issue tracker: Local Markdown

规格和任务保存在仓库的 `.scratch/` 下。

## 文件约定

- 每个功能一个目录：`.scratch/<feature-slug>/`。
- 规格：该目录下的 `spec.md`。
- 任务：该目录下的 `issues/<NN>-<slug>.md`，从 `01` 编号，每个任务单独一个文件。
- 分诊状态：任务文件顶部的 `Status:` 字段；取值见 `triage-labels.md`。
- 评论和讨论：追加到文件末尾的 `## Comments` 下。

## 技能操作

- “发布到事项跟踪系统”：在上述目录中创建对应的规格或任务文件。
- “获取相关任务”：读取用户指定的文件或编号对应的任务。

## Wayfinding

- 地图：`.scratch/<effort>/map.md`，记录笔记、已有决策和待解问题。
- 子任务：同目录下的 `issues/NN-<slug>.md`。
- 类型：`Type:` 字段，取值为 research、prototype、grilling 或 task。
- 执行状态：`Status: claimed` 或 `Status: resolved`。
- 依赖：顶部用 `Blocked by: NN, NN` 记录；所有依赖均 resolved 后才能开始。
- 选取任务：按编号选择首个未解决、未领取且依赖已解除的任务。
- 领取：开始工作前，写入 `Status: claimed`。
- 解决：在 `## Answer` 下追加结果，设为 resolved，并将结论和文件链接追加到地图的决策记录。
