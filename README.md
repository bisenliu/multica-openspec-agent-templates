# Multica OpenSpec Agent Templates

用于在 Multica 中创建围绕 OpenSpec 官方命令工作的智能体模板。

本仓库已经移除旧版“手动生成 OpenSpec proposal/spec/design/tasks”的模板，统一改为官方命令流程：

1. 先在 Multica 中选定要运行的 AI client/runtime。
2. 用户进入目标项目根目录，手动运行 `openspec init`。
3. 后续只通过 OpenSpec 的 `propose`、`apply`、`archive` 三类动作推进 change；具体斜杠命令以当前 AI client 实际注册的命令为准。
4. Multica 智能体负责整理上下文、协调分工、审查风险，并通过 Squad 自动 `@mention` 下游智能体；但不替代 `openspec init`。

## 文档

- [OpenSpec 智能体配置指南](docs/multica-openspec-agent-guide.md)
- [Issue 示例提示词](examples/sample-issue-prompts.md)

## 适用场景

- 在 Multica 中手动创建产品研发智能体。
- 使用 Squad 自动流转 OpenSpec 官方命令。
- Leader 通过精确 `@mention` 自动调用当前阶段需要的成员。
- 将需求上下文、`propose`、`apply`、`archive` 串成稳定协作流程。

## 初始化前提

OpenSpec 初始化不是自动创建步骤，也不应由智能体静默完成。用户必须先手动 init，之后小队才能自动流转。

正确顺序：

```text
在 Multica 选定 AI client/runtime
  -> 用户在目标项目根目录手动运行 openspec init --tools <tool-id>
  -> 记录当前 AI client 的 propose/apply/archive 实际命令
  -> 使用 propose 命令创建 change
  -> 使用 apply 命令实施 change
  -> 使用 archive 命令归档 change
```

示例：

```bash
npm install -g @fission-ai/openspec@latest
openspec init --tools <ai-client-tool-id>
```

如果不确定 tool id，可以运行交互式初始化：

```bash
openspec init
```

## 推荐使用方式

### 推荐智能体

```text
OpenSpec流程协调官
  -> 需求上下文整理官
  -> OpenSpec实施推进官
  -> OpenSpec归档验收官
```

### 命令映射

不同 AI client 的 OpenSpec 命令可能不同，例如 `/opsx:propose`、`/opsx-propose`、`/prompts:opsx-apply`。使用前请记录当前 client 实际可用命令，并在智能体上下文中统一称为：

| 动作 | 占位符 |
| --- | --- |
| 创建变更 | `<PROPOSE_COMMAND>` |
| 实施变更 | `<APPLY_COMMAND>` |
| 归档变更 | `<ARCHIVE_COMMAND>` |

`<PROPOSE_COMMAND>` 这类文本是占位符，不建议让智能体自动猜。推荐在创建小队或复制指令前，根据当前 AI client 的实际命令一次性替换；如果同一个小队要临时切换 client，再在 issue prompt 里传入 `command_map` 覆盖。

Codex 的命令映射：

```yaml
command_map:
  propose: "/prompts:opsx-propose"
  apply: "/prompts:opsx-apply"
  archive: "/prompts:opsx-archive"
```

## 注意事项

如果 Multica project resource 使用 local directory，Multica runtime 可能会在目录根写入 `AGENTS.md`、`CLAUDE.md` 或 `.multica/project/resources.json` 等运行时上下文文件。

如果不希望主仓库被写入，建议：

- 使用独立 git worktree；
- 或使用包装目录；
- 或改用远程 repo resource；
- 不要只依赖提示词阻止写入，因为 runtime 文件通常在 agent 看到提示词之前就已经写入。

## 参考

- [Multica Docs](https://multica.ai/docs)
- [OpenSpec Docs](https://github.com/Fission-AI/OpenSpec)

## License

MIT
