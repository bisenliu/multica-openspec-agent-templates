# Sample Issue Prompts

本文件提供可直接复制到 Multica issue、评论或 chat 中的示例提示词。

## 1. 使用轻量三智能体流程

```text
请按 OpenSpec 轻量流程分析并规划这个需求：

<在这里填写需求>

要求：
- 先做需求澄清和 PRD。
- 再生成 OpenSpec proposal、Delta specs、design 和 tasks。
- 最后给出验收检查点和是否可以 sync/archive 的判断。
```

## 2. 调用需求澄清与PRD策划官

```text
@需求澄清与PRD策划官
请分析这个需求并产出 PRD、用户故事、范围、非范围、成功指标、验收标准和待澄清问题：

<在这里填写需求>
```

## 3. 调用 OpenSpec变更设计官

```text
@OpenSpec变更设计官
请基于上面的 PRD 产出 OpenSpec 变更方案，包含：

- change-id
- proposal.md
- Delta specs
- requirements
- scenarios
- design.md
- tasks.md
```

## 4. 调用交付验收与归档审查官

```text
@交付验收与归档审查官
请基于 PRD、OpenSpec 产物、实现说明和测试结果做验收，判断是否可以 sync/archive。

请输出：
- pass / warning / fail 结论
- OpenSpec requirement 覆盖检查
- PRD 验收标准检查
- tasks 完成度
- 测试结果
- 阻塞问题
- 残余风险
- 归档前必须完成的事项
```

## 5. 使用完整版七智能体流程

```text
请按 OpenSpec 完整流程规划并执行这个需求：

<在这里填写需求>

要求：
- 先由产品需求分析智能体产出 PRD。
- 再由 OpenSpec 规格智能体产出 proposal 和 Delta specs。
- 再由技术设计智能体产出 design.md。
- 再由任务拆解智能体产出 tasks.md。
- tasks.md 确认后再进入开发执行。
- 最后由验收质量智能体做 verify，并给出是否可以 sync/archive 的结论。
```

## 6. 调用 OpenSpec 主协调智能体

```text
@OpenSpec 主协调智能体
请协调以下需求的 OpenSpec 工作流：

<在这里填写需求>

请判断当前应进入哪个阶段，并生成给下一个智能体的提示词。
```

## 7. 调用产品需求分析智能体

```text
@产品需求分析智能体
请基于以下需求做产品需求分析，并输出 PRD、用户故事、范围、非范围、成功指标、验收标准和待澄清问题：

<在这里填写需求>
```

## 8. 调用 OpenSpec 规格智能体

```text
@OpenSpec 规格智能体
请把上面的 PRD 转成 OpenSpec 变更方案，包含 change-id、proposal.md、Delta spec、requirements 和 scenarios。
```

## 9. 调用技术设计智能体

```text
@技术设计智能体
请基于这个 OpenSpec proposal 和 specs 输出技术设计，包括影响范围、接口、数据模型、权限、状态流、风险和测试策略。
```

## 10. 调用任务拆解智能体

```text
@任务拆解智能体
请基于 OpenSpec specs 和 design.md 拆解 tasks.md，要求每项任务可执行、可验证，并标出依赖关系。
```

## 11. 调用开发执行智能体

```text
@开发执行智能体
请按 tasks.md 实施这个 OpenSpec change。每完成一项任务更新状态，并补充必要测试。
```

## 12. 调用验收质量智能体

```text
@验收质量智能体
请根据 PRD、OpenSpec requirements、scenarios、design.md、tasks.md 和代码变更做验收，输出通过、警告或失败结论。
```

## 13. Squad 自动流转提示词

```text
请按 OpenSpec Squad 流程处理这个需求：

<在这里填写需求>

固定阶段：
1. 产品需求分析
2. OpenSpec 规格
3. 技术设计
4. 任务拆解
5. 开发执行
6. 验收质量
7. 主协调总结与 sync/archive 判断

规则：
- 每次只委派给当前阶段最合适的一个成员。
- 不要跳过 OpenSpec 规格阶段。
- 不要在 tasks.md 完成前触发开发执行智能体。
- 如果成员输出包含 blocked、warning、open_questions，先处理阻塞或派回上游修正。
- 委派后停止，等待成员回复触发下一轮。
- 总结、感谢、确认完成时不要 @mention 成员，避免循环触发。
```
