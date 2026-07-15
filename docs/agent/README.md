# Agent 文档索引

本目录集中存放仓库协作、执行门禁和工程闭环文档：

- `AGENTS.md`：贡献规范与项目约定；根目录同名符号链接用于 Codex 自动发现。
- `HARNESS.md`：单次 Agent 执行的权限边界和验证门禁。
- `LOOP.md`：最多三轮的“实现—验证”闭环流程。
- `loop-constraints.md`：每轮必须遵守的强制约束。
- `LOOP_STATE.md`：当前优先级、限制、自动化等级和暂停开关。

仓库级 `$repo-loop` Skill 位于 `.agents/skills/repo-loop/`，其固定目录结构不可移动。
