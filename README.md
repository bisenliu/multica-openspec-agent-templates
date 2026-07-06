# Multica OpenSpec Agent Templates

用于在 Multica 中创建围绕 OpenSpec 官方命令工作的智能体模板。

本仓库主要维护围绕 OpenSpec 官方命令工作的智能体模板，也包含少量不进入 OpenSpec 流程的通用分析智能体。

OpenSpec 模板已经移除旧版“手动生成 OpenSpec proposal/spec/design/tasks”的方式，统一改为官方命令流程：

1. 先在 Multica 中选定要运行的 AI client/runtime。
2. 用户进入目标项目根目录，手动运行 `openspec init`。
3. 后续只通过 OpenSpec 的 `propose`、`apply`、`archive` 三类动作或其子集推进 change；具体斜杠命令以当前 AI client 实际注册的命令为准。
4. Multica 智能体负责整理上下文、协调分工、审查风险，并通过 Squad 自动 `@mention` 下游智能体；但不替代 `openspec init`。

## 文档

- [OpenSpec 公共规则](docs/openspec-common-rules.md)
- [问题排查分析智能体指南](docs/multica-issue-analysis-agent-guide.md)
- [Go 项目架构审计智能体指南](docs/multica-go-architecture-audit-agent-guide.md)
- [OpenSpec 智能体配置指南](docs/multica-openspec-agent-guide.md)
- [Codex 提案实施单智能体指南](docs/multica-openspec-codex-propose-apply-agent-guide.md)
- [Codex 三智能体轻量指南](docs/multica-openspec-codex-3-agent-guide.md)
- [Codex 完整小队指南](docs/multica-openspec-codex-agent-guide.md)
- [Issue 示例提示词](examples/sample-issue-prompts.md)

## 如何选择

OpenSpec 流程先阅读 [OpenSpec 公共规则](docs/openspec-common-rules.md)，再按使用场景选择一份可复制指南；非 OpenSpec 分析任务可直接选择对应智能体：

| 场景 | 推荐文档 | 说明 |
| --- | --- | --- |
| Go 项目企业级架构审查、代码优化评估 | [Go 项目架构审计智能体指南](docs/multica-go-architecture-audit-agent-guide.md) | 1 个通用 agent，输出 Go 架构、性能、安全、可观测性和演进风险审计报告 |
| 只分析问题、整理报告，不生成 OpenSpec 文件 | [问题排查分析智能体指南](docs/multica-issue-analysis-agent-guide.md) | 1 个通用 agent，按用户输入的资料、排除项和输出路径生成 Markdown 报告 |
| Codex 只需要 propose + apply，不需要自动归档 | [Codex 提案实施单智能体指南](docs/multica-openspec-codex-propose-apply-agent-guide.md) | 1 个 agent，只使用 `/prompts:opsx-propose` 和 `/prompts:opsx-apply` |
| Codex 日常需求，追求更少接力和接近 CLI 的速度 | [Codex 三智能体轻量指南](docs/multica-openspec-codex-3-agent-guide.md) | 3 个 agent，无额外 Leader，入口直接 `@OpenSpec提案官` |
| Codex 复杂需求、需求不清楚或风险较高 | [Codex 完整小队指南](docs/multica-openspec-codex-agent-guide.md) | 4 个 agent，包含流程协调和需求上下文整理 |
| 非 Codex runtime，或同一套模板要适配不同 AI client | [OpenSpec 智能体配置指南](docs/multica-openspec-agent-guide.md) | 使用 `command_map` 和命令占位符 |
| 手动触发或 issue/chat 里复制提示词 | [Issue 示例提示词](examples/sample-issue-prompts.md) | 提供分阶段和自动流程示例 |

## 适用场景

- 在 Multica 中手动创建产品研发智能体。
- 使用 Squad 自动流转 OpenSpec 官方命令。
- 使用通用分析智能体整理问题排查报告。
- 使用 Go 架构审计智能体评估企业级生产就绪度和长期演进风险。
- Leader 通过精确 `@mention` 自动调用当前阶段需要的成员。
- 将需求上下文、`propose`、`apply`、`archive` 串成稳定协作流程。

## 初始化前提

OpenSpec 初始化、命令边界、串行并发、propose 证据门禁和 Multica runtime 文件处理统一维护在 [OpenSpec 公共规则](docs/openspec-common-rules.md) 中。问题排查、Go 架构审计等通用分析智能体不需要 `openspec init`。

OpenSpec 流程最短正确顺序：

正确顺序：

```text
在 Multica 选定 AI client/runtime
  -> 用户在目标项目根目录手动运行 openspec init --tools <tool-id>
  -> 记录当前 AI client 的 propose/apply/archive 实际命令或所选流程需要的命令子集
  -> 使用 propose 命令创建 change
  -> 使用 apply 命令实施 change
  -> 如果所选流程包含归档，再使用 archive 命令归档 change
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

### 默认选择

- 只做问题排查和 Markdown 报告时，使用 [问题排查分析智能体指南](docs/multica-issue-analysis-agent-guide.md)。
- 做 Go 项目企业级架构审查时，使用 [Go 项目架构审计智能体指南](docs/multica-go-architecture-audit-agent-guide.md)。
- 只需要 propose 和 apply 时，使用 [Codex 提案实施单智能体指南](docs/multica-openspec-codex-propose-apply-agent-guide.md)。
- 需要完整 propose、apply、archive 自动流转时，使用 [Codex 三智能体轻量指南](docs/multica-openspec-codex-3-agent-guide.md)。
- 需求复杂、上下文不清楚或需要更强审查时，使用 [Codex 完整小队指南](docs/multica-openspec-codex-agent-guide.md)。
- 使用其他 AI client/runtime 时，使用 [OpenSpec 智能体配置指南](docs/multica-openspec-agent-guide.md)。

### 命令映射

不同 AI client 的 OpenSpec 命令可能不同，例如 `/opsx:propose`、`/opsx-propose`、`/prompts:opsx-apply`。使用前请记录当前 client 实际可用命令，并在智能体上下文中统一称为：

| 动作 | 占位符 |
| --- | --- |
| 创建变更 | `<PROPOSE_COMMAND>` |
| 实施变更 | `<APPLY_COMMAND>` |
| 归档变更 | `<ARCHIVE_COMMAND>` |

`<PROPOSE_COMMAND>` 这类文本是占位符，不建议让智能体自动猜。推荐在创建小队或复制指令前，根据当前 AI client 的实际命令一次性替换；如果同一个小队要临时切换 client，再在 issue prompt 里传入 `command_map` 覆盖。

Codex 完整三段式命令映射：

```yaml
command_map:
  propose: "/prompts:opsx-propose"
  apply: "/prompts:opsx-apply"
  archive: "/prompts:opsx-archive"
```

如果使用 Codex 提案实施单智能体，只需要前两条命令，不使用 `archive`。

## 注意事项

OpenSpec 相关指南共享的注意事项见 [OpenSpec 公共规则](docs/openspec-common-rules.md)。重点包括：agent 不替代 `openspec init`、所有相关 agent 和 Squad 并发设为 `1`、没有 OpenSpec change 文件证据不得进入 `apply`、归档前排除 Multica runtime 文件影响。

问题排查分析、Go 架构审计等通用分析智能体独立于 OpenSpec 流程，只按用户指定资料、审查范围、排除项和输出路径生成 Markdown 报告。

## 参考

- [Multica Docs](https://multica.ai/docs)
- [OpenSpec Docs](https://github.com/Fission-AI/OpenSpec)

## License

MIT
