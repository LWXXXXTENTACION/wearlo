# 仓库贡献指南

## 项目结构与模块组织

本仓库用于构建一个基于 Agent 的电商平台，目前处于空项目骨架阶段。新增实现时，根目录只保留入口、配置和说明文件；应用代码放在 `src/`，测试放在 `tests/`，静态资源放在 `assets/`，补充文档放在 `docs/`。测试目录应镜像源码路径，例如 `src/services/catalog.ts` 对应 `tests/services/catalog.test.ts`。按业务功能拆分模块，避免不断扩大的通用工具文件。

## 构建、测试与本地开发

当前尚未配置语言、包管理器或构建系统，因此没有可执行的应用命令。引入技术栈时，应同时补充 README、`.gitignore` 和 `docs/agent/HARNESS.md` 中的验证门禁，并提供统一入口，例如：

- `npm run dev`：启动本地开发服务。
- `npm test`：运行完整测试。
- `npm run build`：生成生产构建。
- `npm run lint`：执行静态检查和格式检查。

未在项目配置中声明的命令不得视为可用。不要提交依赖目录、构建产物或本地缓存。

## 编码风格与命名

优先遵循仓库内格式化器和 linter。工具落地前，JSON、YAML、JavaScript 和 TypeScript 使用两空格缩进，文件采用 UTF-8 并以换行结束。变量和函数使用 `camelCase`，类与 UI 组件使用 `PascalCase`，目录及非组件文件使用 `kebab-case`。函数保持单一职责，公共接口需写清用途和边界。

## 测试规范

每次行为变更和缺陷修复都应附带测试。测试文件采用 `*.test.*`，固定数据放在 `tests/fixtures/`。测试必须可重复、彼此隔离，默认不得依赖真实外部服务。引入覆盖率工具后，优先覆盖关键分支，不以单一百分比替代有效断言。

## Harness 与 Loop

执行仓库任务前先阅读 `docs/agent/HARNESS.md`、`docs/agent/LOOP.md`、`docs/agent/loop-constraints.md` 和 `docs/agent/LOOP_STATE.md`。需要有界的“实现—验证”闭环时使用 `$repo-loop <任务>`；单次任务最多尝试三轮，并遵守人工审批边界。

涉及远程仓库、Issue 或 PR 时使用 `@github` 连接器。当前 `origin` 为 `LWXXXXTENTACION/wearlo`；优先通过连接器读取结构化信息，目标与 `origin` 不一致时必须由用户明确指定，不得猜测。创建 Issue、评论、修改 PR 等远程写操作必须在任务中得到明确授权。

## 提交与 Pull Request

当前目录没有 Git 历史可供归纳。提交标题使用简短祈使句，如 `Add catalog search endpoint`；建议采用 `feat:`、`fix:`、`docs:` 等 Conventional Commits 前缀。PR 必须说明问题、方案和验证结果，关联相关 issue；可见界面变更需附截图，配置或迁移变化需写明操作步骤。

## 安全与配置

禁止提交密钥或机器专属配置。使用 `.env.example` 提供脱敏模板，在启动时校验必填配置，并在引入依赖前检查来源和必要性。
