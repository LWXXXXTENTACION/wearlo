# 仓库工程闭环

本仓库采用与 `chatbot` 项目一致的 Codex 原生有界“实现者—验证者”闭环。当前等级为 **L1 辅助模式**：人工提供单个任务，Codex 在当前终端实现并验证，所有改动由人工复核。

## 执行流程

1. 阅读根目录 `AGENTS.md`，以及 `docs/agent/HARNESS.md`、`docs/agent/loop-constraints.md` 和 `docs/agent/LOOP_STATE.md`。
2. 检查任务和现有工作区状态，保留无关改动；需要远程上下文时用 `@github` 读取。
3. 做最小且完整的修改，不提交、不推送。
4. 运行 `docs/agent/HARNESS.md` 中适用的快速检查，只修复本任务引起的失败。
5. 报告 `DONE` 前运行全部适用的最终门禁。
6. 成功、命中约束或达到三次尝试上限时停止。

## 使用方式

在本仓库的交互式 Codex 终端中输入：

```text
$repo-loop 初始化项目的基础目录和测试框架
$repo-loop 只读排查当前仓库风险，不修改文件
$repo-loop 使用 @github 汇总 owner/repo 的待处理 Issue，仅报告
```

仓库级 Skill 位于 `.agents/skills/repo-loop/SKILL.md`，结果为 `DONE`、`RETRY` 或 `BLOCKED`。

## 人工门禁

提交、推送、安装依赖、GitHub 远程写操作、产生其他外部副作用、修改认证或持久化数据模型，以及 `docs/agent/loop-constraints.md` 指定的操作，均需人工批准。自动化不得合并、部署、编辑秘密信息或丢弃工作区改动。

## 状态与可观测性

`docs/agent/LOOP_STATE.md` 保存共享优先级、已知限制和暂停开关。仅在人工审阅一次运行后更新共享状态。
