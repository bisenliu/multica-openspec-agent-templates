# Sample Issue Prompts

本文件提供可直接复制到 Multica issue、评论或 chat 中的示例提示词。

公共前提和硬约束见 [OpenSpec 公共规则](../docs/openspec-common-rules.md)。本文件保留可直接复制的提示词，因此会重复少量关键规则。

## 1. 使用前检查

```text
开始使用小队前，用户先自行确认：

1. Multica agent runtime 已选定为本次要使用的 AI client。
2. 已在目标项目根目录手动运行：

openspec init --tools <tool-id>

如果不确定 tool id，先运行交互式：

openspec init

3. 已确认当前 AI client 的命令映射：

command_map:
  propose: "<PROPOSE_COMMAND>"
  apply: "<APPLY_COMMAND>"
  archive: "<ARCHIVE_COMMAND>"

说明：这里的 <PROPOSE_COMMAND> / <APPLY_COMMAND> / <ARCHIVE_COMMAND> 是占位符。固定使用一个 AI client 时，建议在创建小队或复制智能体指令前一次性替换成真实命令；不建议让智能体自动猜。

Codex 示例：

command_map:
  propose: "/prompts:opsx-propose"
  apply: "/prompts:opsx-apply"
  archive: "/prompts:opsx-archive"

确认完成后，再把需求交给 OpenSpec 官方命令小队。
```

## 2. 整理需求上下文

```text
@需求上下文整理官
请整理以下需求，输出可交给 <PROPOSE_COMMAND> 的上下文。

<在这里填写需求>

要求：
- 输出目标、范围、非范围、验收标准、风险和待澄清问题。
- 建议一个 kebab-case change-id。
- 不要手写 proposal.md、specs、design.md 或 tasks.md。
- 不要仿写 OpenSpec 文件结构。
```

## 3. 准备 propose

```text
@OpenSpec流程协调官
请基于上面的需求上下文直接推进 propose 动作。

command_map:
  propose: "<PROPOSE_COMMAND>"
  apply: "<APPLY_COMMAND>"
  archive: "<ARCHIVE_COMMAND>"

要求：
- 只使用 command_map.propose 对应的实际命令。
- 不要手写 OpenSpec 产物。
- 如果当前环境不能执行 propose 命令，请输出 blocked 并说明原因；不要把命令只作为用户待办。
- propose 成功后必须输出 created_change_path 和 created_files。
- 如果没有生成 openspec/changes/<change-id>/proposal.md、tasks.md 和至少一个 specs/<capability>/spec.md，不要进入 apply。
```

## 4. 推进 apply

```text
@OpenSpec实施推进官
change-id 是 <change-id>。
propose 已完成，OpenSpec 生成的变更已经确认可以进入实现。

command_map:
  propose: "<PROPOSE_COMMAND>"
  apply: "<APPLY_COMMAND>"
  archive: "<ARCHIVE_COMMAND>"

请直接推进 apply 实现阶段。

要求：
- 只围绕已确认的 change 范围推进。
- 不要修改需求范围。
- 开始前必须确认 openspec/changes/<change-id>/ 存在，且 proposal.md、tasks.md 和至少一个 specs/<capability>/spec.md 已生成。
- 只围绕 command_map.apply 对应的实际命令推进，不引入其他 OpenSpec 动作。
- 如果当前环境不能执行 apply 命令，请输出 blocked 并说明原因；不要把命令只作为用户待办。
```

## 5. 归档前检查

```text
@OpenSpec归档验收官
change-id 是 <change-id>。
apply 已完成，以下是实现说明和测试结果：

command_map:
  propose: "<PROPOSE_COMMAND>"
  apply: "<APPLY_COMMAND>"
  archive: "<ARCHIVE_COMMAND>"

<填写实现说明和测试结果>

要求：
- 判断是否满足 archive 条件。
- 列出阻塞问题、警告和必须修复项。
- 只围绕 command_map.archive 对应的实际命令判断归档条件，不引入其他 OpenSpec 动作。
- 如果项目验证命令包含整仓库 git diff/status 检查，不要把 Multica runtime 自动生成或临时修改的 AGENTS.md（与 openspec/ 同级）、CLAUDE.md、.multica/project/resources.json、.multica/** 导致的工作区不干净作为 archive 阻塞；请排除这些运行期文件或限定检查源码、测试和 openspec/changes/<change-id>/ 相关路径。
- 如果满足归档条件，请直接执行 archive 命令。
- 如果当前环境不能执行 archive 命令，请输出 blocked 并说明原因；不要把命令只作为用户待办。
```

## 6. 使用 Squad 自动流程

```text
@OpenSpec流程协调官
请按 OpenSpec 官方命令小队的自动流程处理这个需求：

command_map:
  propose: "<PROPOSE_COMMAND>"
  apply: "<APPLY_COMMAND>"
  archive: "<ARCHIVE_COMMAND>"

需求：
<在这里填写需求>

规则：
- 不要手写 OpenSpec 文件。
- 只使用 command_map 中的 propose、apply、archive 三类动作命令。
- propose 成功后必须输出 created_change_path 和 created_files；没有 OpenSpec 文件证据，不得进入 apply。
- archive 前不要被 Multica runtime 自动生成文件或 AGENTS.md（与 openspec/ 同级）临时改写造成的整仓库 diff/status 失败卡住；排除 AGENTS.md、CLAUDE.md、.multica/project/resources.json、.multica/** 后再判断。
- 需求不清楚时，@需求上下文整理官。
- 需求清楚后由 @OpenSpec流程协调官 执行 command_map.propose。
- propose 完成后，@OpenSpec实施推进官。
- apply 完成后，@OpenSpec归档验收官。
- 如果成员输出包含 blocked、warning、open_questions，先处理阻塞或派回上游修正。
- @mention 下游后停止，等待成员回复或平台重新触发。
- 总结、感谢、确认完成时不要 @mention 成员，避免循环触发。
```

## 7. Codex 单智能体 propose + apply

```text
@OpenSpec提案实施官

请按 OpenSpec Codex 提案实施流程处理这个需求，只使用：
- /prompts:opsx-propose
- /prompts:opsx-apply

需求：
<在这里填写需求>

可选 change-id：
<如果没有就留空，由 OpenSpec提案实施官生成>

要求：
- 不调用 /prompts:opsx-archive。
- 不手写 OpenSpec 文件。
- propose 成功后必须输出 created_change_path 和 created_files；没有 OpenSpec 文件证据不得进入 apply。
- apply 完成后运行必要测试并停止，不做归档。
```
