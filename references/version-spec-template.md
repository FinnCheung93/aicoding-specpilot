# Version Specs Template

`Version Specs` 是 `aicoding-specpilot` 的 PRD-to-SDD 主产物。它把 PRD 中的版本意图转成 AI Coding 可读取、可实现、可验收、可审查的产品侧执行契约。

它不替代 PRD。PRD 仍是版本需求权威；Version Specs 是派生规范。冲突时回到 PRD 裁决，或先更新 PRD 再同步 Specs。

## 1. 使用前检查

生成前先确认：

- 用户明确要求基于 PRD / 等价版本需求进入 Specs 生成。
- PRD source 已识别。
- Language Lock 已完成。
- PRD Readiness Check 覆盖版本目标、目标用户、范围边界、用户路径、验收口径。
- PRD Coverage Pass 已完成。
- 已明确 version id，例如 `v0.1`、`phase-0.8`、`mvp`；若用户未给，使用 PRD 文件名或目标版本名称推断，并标为 assumption。

## 2. 推荐目录

```text
SPECS/
  versions/
    <version>/
      README.md
      requirements.md
      flows.md
      states.md
      acceptance.md
      edge-cases.md
      coverage.md
      base-candidates.md
      handoff.md
```

这是默认检查面，不是机械文件数量要求。小版本可以合并文件，但必须覆盖同等问题；没有结论的部分标为 pending 或 not-applicable。

## 3. README.md

用途：版本规格入口。

建议内容：

- Version id。
- Status：Draft / Review / Ready for AI Coding / Approved。
- PRD source。
- PRD authority。
- 当前生成模式：PRD-first / assumption-based v0。
- 需要一起读取的 Base Specs。
- 本版本文档阅读顺序。
- P0/P1 审查状态。
- Pending / conflicts 摘要。

## 4. requirements.md

用途：说明本版本产品侧目标和范围，不复制完整 PRD。

建议章节：

- Version Goal。
- Target Users。
- In Scope。
- Out of Scope。
- Deferred。
- Product Constraints。
- Non-goals。
- Open Decisions。

注意：PRD 中的背景、市场、长篇解释可引用，不要全文搬运。

## 5. flows.md

用途：定义本版本实际启用或验证的用户路径。

建议每条 Flow 包含：

- Flow name。
- Trigger。
- Actor。
- Preconditions。
- Main path。
- Alternate / failure path。
- Exit condition。
- Related acceptance。

只写本版本需要实现、验证或影响体验的路径。长期通用路径原则放入 Base。

## 6. states.md

用途：定义本版本必须处理的状态和状态转换。

建议每个状态包含：

- State name / id。
- User-visible meaning。
- Trigger。
- Allowed actions。
- Exit / transition。
- Blocking level。
- Copy / feedback requirement。

不要把纯技术内部状态写成产品状态；工程侧内部状态放 handoff。

## 7. acceptance.md

用途：定义产品侧验收场景。

可使用 Given / When / Then，也可使用等价结构。关键是可判定。

建议覆盖：

- Happy path。
- Critical failure path。
- 权限 / 边界。
- 空态 / 加载 / 超时。
- 用户取消或撤回。
- 文案和状态可见性。
- 不通过条件。

示例：

```md
### Scenario: 用户完成一次核心任务

Given 用户已满足前置条件  
When 用户执行核心操作  
Then 系统应展示明确的完成状态  
And 用户能查看本次任务结果
```

## 8. edge-cases.md

用途：定义本版本必须裁决的异常、边界和失败场景。

建议每条包含：

- Case。
- Trigger。
- User impact。
- Expected product behavior。
- Copy / state requirement。
- Acceptance impact。
- Handoff need。

只写用户可感知或产品必须有结论的情况，不写完整测试矩阵。

## 9. coverage.md

用途：回答“PRD 中的关键要求去哪了”。

推荐表格：

```md
| PRD 要点 | 状态 | 去向 / 原因 |
|---|---|---|
| ... | version-spec / base / kept-in-prd / pending / conflict / handoff | ... |
```

处置状态：

- `version-spec`：进入当前 Version Specs。
- `base`：抽取为长期 Base 规则。
- `base-candidate`：可能提升 Base，但需要确认。
- `kept-in-prd`：只属于当前 PRD，不复制进 Specs。
- `pending`：信息不足。
- `conflict`：材料冲突。
- `handoff`：交给工程、测试、安全或架构后续文档。
- `excluded-with-reason`：不适合进入产品侧 Specs，并已说明原因。

## 10. base-candidates.md

用途：记录可能未来提升为 Base 的候选，禁止静默 Promote。

建议每条包含：

- Candidate。
- Source。
- Why reusable。
- Affected Base Spec。
- Risk of promotion。
- Recommended decision：promote / defer / reject。
- Decision status：pending / accepted / rejected / deferred。

Promote 条件：

- 跨版本稳定成立。
- 跨 AI Coding 行动稳定复用。
- 不依赖单个 PRD 的临时范围或取舍。
- 用户或产品 owner 明确确认。

## 11. handoff.md

用途：说明研发、架构、测试、安全或 AI Coding 后续需要补什么。

建议分类：

- Engineering architecture。
- API / events。
- Data model。
- Test cases / test strategy。
- Security / permission review。
- Implementation plan / tasks。
- Open questions for non-product teams。

这里是 handoff，不替专业团队直接做决策。
