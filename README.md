# Multica OpenSpec Agent Templates

用于在 Multica 中创建 OpenSpec 产品研发智能体的 Markdown 模板集合。

本仓库提供两套配置：

- **7 智能体完整版**：覆盖产品需求分析、OpenSpec 规格、技术设计、任务拆解、开发执行、验收质量与主协调。
- **3 智能体轻量版**：覆盖需求澄清、OpenSpec 变更设计与交付验收。

## 文档

- [完整版智能体配置](docs/multica-agent-manual-openspec-prd.md)
- [轻量三智能体配置](docs/multica-agent-lite-3-openspec.md)
- [Issue 示例提示词](examples/sample-issue-prompts.md)

## 适用场景

- 在 Multica 中手动创建产品研发智能体。
- 使用 `@mention` 按阶段调用智能体。
- 使用 Squad 自动流转 OpenSpec 工作流。
- 将 PRD、OpenSpec proposal、Delta specs、design、tasks、verify 串成稳定协作流程。

## 推荐使用方式

### 轻量版

适合个人项目、早期探索或需求规模较小的场景。

```text
需求澄清与PRD策划官
  -> OpenSpec变更设计官
  -> 交付验收与归档审查官
```

### 完整版

适合多人协作、复杂需求、长期项目或需要明确质量门禁的场景。

```text
OpenSpec 主协调智能体
  -> 产品需求分析智能体
  -> OpenSpec 规格智能体
  -> 技术设计智能体
  -> 任务拆解智能体
  -> 开发执行智能体
  -> 验收质量智能体
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
- [OpenSpec 中文文档](https://radebit.github.io/OpenSpec-Docs-zh/#overview)

## License

MIT
