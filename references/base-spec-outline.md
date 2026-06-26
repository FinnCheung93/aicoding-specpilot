# Base Specs 大纲

Base Specs 是项目级、长期稳定的产品侧规范。它回答“这个产品长期是什么、有何原则、用户如何理解和使用、哪些边界不能被版本需求随意突破”。

在 PRD-first / Version-first 工作流中：

- PRD 是版本需求权威。
- Version Specs 是当前版本执行契约。
- Base Specs 是从多个 PRD / Version Specs 中抽象出的稳定规则。

Base Specs 不等于某个版本 PRD，也不替代研发、测试、安全文档。

## 1. 生成原则

- 正文语言遵循 Language Lock。
- 文件名保持英文，方便 AI Coding Agent 检索。
- 每份文档只承担一个清晰职责。
- 不把 PRD 的一次性版本范围写成长期规则。
- 不机械填满章节；没有结论的内容标为 pending。
- 引用输入材料时区分事实、推断、假设和冲突。
- 当前版本内容优先进入 `SPECS/versions/<version>/`。
- 可能长期复用但尚未确认的内容标为 `base-candidate`，不要静默 Promote。

## 2. 推荐目录

```text
SPECS/
  base/
    product-definition.md
    requirements-and-boundaries.md
    user-flows.md
    states.md
    copy-guidelines.md
    edge-cases.md
    acceptance-principles.md
    delivery-handoff.md
```

Base-only 模式可以只生成 `SPECS/README.md` + `SPECS/base/`。PRD-to-AI-Coding 默认还应生成 `SPECS/versions/<version>/`。

## 3. product-definition.md

用途：定义产品是什么、不是什么、核心价值和长期边界。

建议章节：

- 产品定义。
- 产品不是。
- 核心用户角色。
- 核心价值。
- 产品原则。
- 长期能力边界。
- 关键术语。
- 待决问题。

注意：这里写长期稳定判断，不写某个版本必须交付的全部内容。

## 4. requirements-and-boundaries.md

用途：定义长期业务目标、问题空间、范围边界和成功原则。

建议章节：

- 背景与问题。
- 长期业务目标。
- 长期需求范围。
- 非目标。
- 成功原则。
- 约束。
- 待决问题。

注意：如果某项只属于当前 PRD 版本，进入 Version Specs 或保留在 PRD；不要提升为 Base。

## 5. user-flows.md

用途：定义可复用用户路径，帮助 Agent 在实现流程、CLI、Web 或 Desktop 体验时读取。

建议章节：

- Flow 目录。
- 首次配置路径。
- 任务发起路径。
- 计划确认路径。
- 执行中查看路径。
- 结果与 Artifact 查看路径。
- 失败、取消、超时路径。
- 未来承接路径。

注意：Base 中写通用路径和路径原则；某版本实际启用哪些路径由 Version Specs 决定。

## 6. states.md

用途：定义产品可感知状态词典，避免 UI、CLI、日志和验收使用不同语言。

建议章节：

- 状态命名规则。
- 环境状态。
- 配置状态。
- 任务状态。
- 审批状态。
- Artifact 状态。
- 错误状态。
- 状态转换注意事项。

每个状态建议包含：状态名、用户可见含义、触发条件、用户可执行动作、是否阻断继续。

## 7. copy-guidelines.md

用途：定义关键文案原则和可复用提示语。

建议章节：

- 语气原则。
- 命名规则。
- 成功提示。
- 失败提示。
- 高风险确认。
- 策略拒绝。
- 审批等待。
- 安全提示。
- 术语表。

注意：这里不需要写完整 UI 文案表，但应给 Agent 足够规则，避免高风险场景写得含糊。

## 8. edge-cases.md

用途：定义产品必须有长期结论的边界情况。

建议章节：

- 权限与越权。
- 输出目录与文件边界。
- 高风险操作。
- Agent plan 不合理。
- 用户确认后失败。
- 外部依赖卡死。
- 网络中断。
- 取消失败。
- 日志或输出包含敏感信息。
- 状态不一致。

注意：版本特定 edge cases 放在 Version Specs；Base 只写长期产品期望和体验原则。

## 9. acceptance-principles.md

用途：定义产品侧验收原则，帮助版本验收和测试团队后续扩展。

建议章节：

- 验收目标。
- 用户路径验收。
- 状态验收。
- 文案验收。
- 边界情况验收。
- 安全感知验收。
- 不通过条件。

注意：这不是测试用例全集。Version Specs 负责当前版本的具体验收场景；Base 只记录可复用的验收原则。

## 10. delivery-handoff.md

用途：说明产品侧 Specs 交付给研发、架构、测试、安全和 AI Coding 时，还建议补哪些专业文档。

建议章节：

- 给研发的建议文档。
- 给架构的建议文档。
- 给测试的建议文档。
- 给安全的建议文档。
- 给 AI Coding Agent 的读取建议。
- 当前不由产品侧 Specs 决定的事项。

注意：handoff 是建议清单，不替这些团队做专业决策。
