---
name: repo-loop
description: 在当前仓库运行有界的实现者—验证者编码闭环。用户调用 $repo-loop，或要求对一个范围明确的仓库任务进行反复实现、修复、重构、验证或只读排查时使用；单次最多尝试三轮。
---

# 仓库闭环

将 `$repo-loop` 后的文本视为任务；仅在没有任务内容时询问用户。

## 准备

1. 完整阅读根目录 `AGENTS.md`，以及 `docs/agent/HARNESS.md`、`docs/agent/LOOP.md`、`docs/agent/loop-constraints.md` 和 `docs/agent/LOOP_STATE.md`。
2. 若 `docs/agent/LOOP_STATE.md` 中 `loop-pause: true`，立即停止。
3. 运行 `git status --short` 并保留已有改动；若仓库尚未初始化 Git，只记录该情况，不要擅自初始化。
4. 任务涉及 GitHub 时，优先使用 `@github` 获取仓库、Issue 或 PR 信息。从本地 remote 或用户输入解析 `owner/repo`；无法解析时询问用户，不得猜测。
5. 定义可验证的完成条件。每次只处理一个任务，最多进行三次实现尝试。

## 迭代

每次尝试执行：

1. 检查相关代码、配置和历史，确认任务边界。
2. 做最小且完整的修改，不触碰受保护路径或无关文件。
3. 直接在终端运行 `docs/agent/HARNESS.md` 中适用的快速检查。
4. 若修改导致检查失败，先定位原因再重试；不得削弱或跳过门禁。
5. 达成完成条件后，运行 `docs/agent/HARNESS.md` 中全部适用的最终检查。

约束要求人工决策时立即返回 `BLOCKED`。仅在下一轮能处理明确故障时返回 `RETRY`；三次失败后必须停止。

## 报告

以 `DONE`、`BLOCKED` 或 `RETRY` 开头，并列出：

- 已使用的尝试次数；
- 修改的文件；
- 验证命令及结果；
- 剩余警告或准确的阻塞原因。

`@github` 默认只读。不得擅自创建或修改远程内容，也不得提交、推送、合并、部署、安装依赖、编辑秘密信息、丢弃现有改动或访问生产服务。
