# OpenSpec 公共规则

本文收纳所有 Multica OpenSpec 小队共用的规则。各配置指南只说明具体小队形态和命令差异。

## 使用前提

OpenSpec 初始化不是 agent 自动创建步骤。用户必须先在 Multica 中选定本次运行的 AI client/runtime，再进入目标项目根目录手动初始化。

```bash
npm install -g @fission-ai/openspec@latest
openspec init --tools <ai-client-tool-id>
```

如果不确定 tool id，可以运行交互式初始化：

```bash
openspec init
```

初始化后应记录当前 AI client 实际可用的三类命令，或所选流程需要的命令子集：

```yaml
command_map:
  propose: "<PROPOSE_COMMAND>"
  apply: "<APPLY_COMMAND>"
  archive: "<ARCHIVE_COMMAND>"
```

Codex 完整三段式命令为：

```yaml
command_map:
  propose: "/prompts:opsx-propose"
  apply: "/prompts:opsx-apply"
  archive: "/prompts:opsx-archive"
```

如果所选模板只覆盖 propose + apply，则只使用前两条命令，并明确不归档。

如果后续更换 AI client/runtime，应重新确认 OpenSpec 是否已为新工具完成初始化。

## 命令边界

- 模板只在 `propose`、`apply`、`archive` 动作集合内选择；部分轻量模板可以只使用 `propose` 和 `apply`，并明确不归档。
- Agent 不要手写 `proposal.md`、Delta specs、`design.md`、`tasks.md` 来替代 OpenSpec 命令。
- 当上下文足够且环境允许时，当前阶段 agent 应直接推进对应命令，不要只把命令作为用户待办输出。
- `<PROPOSE_COMMAND>`、`<APPLY_COMMAND>`、`<ARCHIVE_COMMAND>` 是占位符，不应由 agent 自动猜测。
- Codex 的 `/prompts:opsx-*` 是 Codex prompt/slash command，由 Codex runtime 处理；不要因为 API 层没有单独的 slash command executor 就标记 blocked。

## 串行协作

OpenSpec 小队是串行流程。所有相关 agent 和 Squad 的并发建议设为 `1`，避免 `propose`、`apply`、`archive` 乱序或多个成员同时修改同一批文件。

自动接力时，每次只 `@mention` 一个下游成员；`@mention` 后停止，等待成员回复或平台重新触发。总结、感谢、确认完成时不要 `@mention`，避免循环触发。

## Propose 证据门禁

`propose` 成功后必须确认 `openspec/changes/<change-id>/` 已创建，并输出：

```yaml
created_change_path:
created_files:
  -
```

进入 `apply` 前必须确认至少存在：

- `openspec/changes/<change-id>/proposal.md`
- `openspec/changes/<change-id>/tasks.md`
- 至少一个 `openspec/changes/<change-id>/specs/<capability>/spec.md`

如果没有这些文件证据，不得进入 `apply`，也不得 `@mention` 实施阶段 agent。

## Apply 与 Archive 检查

`apply` 只处理当前 OpenSpec change 范围内的实现。修改代码后应运行项目对应的格式化、测试或验证命令；无法运行时说明原因。

`archive` 前应检查实现说明、测试结果、任务完成情况和残余风险。只有源码、测试、OpenSpec change 产物存在未解释问题，或验证命令本身失败时，才阻止 archive。

## Multica Runtime 文件

如果 Multica project resource 使用 local directory，Multica runtime 可能会在目录根写入或临时修改：

- `AGENTS.md`
- `CLAUDE.md`
- `.multica/project/resources.json`
- `.multica/**`

这些文件属于 Multica 执行上下文，任务完成时可能被 Multica 自动删除。归档前验证不要把整仓库 `git diff --exit-code`、`git status --porcelain` 或包含这类检查的 `make lint verify` 作为硬门禁；应排除上述 runtime 文件，或只检查源码、测试和 `openspec/changes/<change-id>/` 相关路径。

仅有 Multica runtime 文件导致工作区不干净时，应记录为 warning，不应阻止 OpenSpec archive。

如果不希望主仓库被写入，建议使用独立 git worktree、包装目录或远程 repo resource。不要只依赖提示词阻止写入，因为 runtime 文件通常在 agent 看到提示词之前就已经写入。
