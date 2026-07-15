# Agent 执行框架

本文件定义单次 Codex 执行的上下文、权限和验证门禁；仓库级 `$repo-loop` 技能负责在需要时重复执行。

## 上下文

- 当前仓库仅包含协作与闭环配置，尚无应用源码、依赖清单和构建系统。
- 开始编辑前必须阅读根目录 `AGENTS.md`，以及 `docs/agent/LOOP.md`、`docs/agent/loop-constraints.md` 和 `docs/agent/LOOP_STATE.md`。
- GitHub 插件已连接；远程仓库、Issue 和 PR 上下文优先使用 `@github`，本地分支状态使用 `git` 或必要时使用 `gh`。
- 引入技术栈时，必须在同一变更中把真实的开发、测试、lint 和构建命令补充到本文件。

## 权限与边界

Agent 可检查整个仓库，并为已分配任务编辑仓库内文件。任务需要 GitHub 上下文时，可通过 `@github` 进行只读查询；没有本地 remote 或明确的 `owner/repo` 时必须询问目标。未经人工明确批准，不得执行 GitHub 写操作、提交或推送、安装依赖、访问生产服务、执行破坏性命令，也不得修改秘密信息或覆盖无关的既有改动。

## 验证契约

当前配置变更至少运行：

```bash
test -L AGENTS.md && test -s docs/agent/AGENTS.md
test -s docs/agent/HARNESS.md && test -s docs/agent/LOOP.md
test -s docs/agent/loop-constraints.md && test -s docs/agent/LOOP_STATE.md
```

如果环境中存在 Skill 校验器，还应运行：

```bash
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py" .agents/skills/repo-loop
```

技术栈落地后，只运行项目已实际配置的门禁。例如存在对应脚本和依赖时运行 `npm test`、`npm run lint`、`npm run build`；Python 项目配置 pytest 后运行 `python3 -m pytest`。不要用假设命令制造通过结果。

任务仅在目标行为已实现、受保护路径未变、所有适用门禁通过，且最终报告列出修改文件和验证证据时完成。依赖缺失、产品决策含糊、安全或数据模型变化、以及三次失败均需升级给人工处理。
