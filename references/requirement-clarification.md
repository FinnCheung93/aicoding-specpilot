# PRD Readiness Check

这个 reference 定义 `aicoding-specpilot` 的 PRD-first 前置检查。目标不是替代 PRD 做完整需求澄清，而是判断已有 PRD 或等价版本需求是否足够支撑产品侧 Specs 派生。

## 1. 核心定义

PRD Readiness Check 要回答：

- PRD 来源是什么，是否足以作为版本需求权威。
- 当前版本目标是否清楚。
- 目标用户、评审者和受影响对象是否清楚。
- 本版本范围、非目标和暂缓项是否清楚。
- 用户路径是否足够支撑 `03-user-flows.md` 的抽象。
- 产品侧验收口径是否足够支撑 `07-acceptance-principles.md` 的抽象。

如果这些内容缺失、冲突或只能由 Agent 推断，默认报告 PRD gaps，并建议回到 PRD 流程补齐。

## 2. 标准步骤

在生成或重写任何 Specs 前，必须完成以下步骤，除非用户明确要求 `assumption-based v0`：

1. **PRD Source Detection**：识别 PRD、版本需求文档或等价输入材料。
2. **Language Lock**：确认正文语言、文件名语言和稳定术语规则。
3. **Readiness Scan**：检查版本目标、目标用户、范围边界、用户路径、验收口径。
4. **Gap Detection**：区分 confirmed、assumption、pending、conflict。
5. **PRD Readiness Summary**：生成前输出 readiness 判断。
6. **PRD Coverage Pass**：判断 PRD 关键内容的去向，避免信息静默丢失。
7. **Write Authorization**：如需写文件，再确认写入授权。

写文件授权不能替代 PRD Readiness Check。只问“是否继续生成 / 是否写文件 / 是否按判断执行”，不算完成 readiness。

## 3. 最低 Ready 条件

| 类别 | 必须从 PRD 中识别什么 |
|---|---|
| 版本目标 | 本版本要达成、验证或交付什么 |
| 目标用户 | 谁使用、谁评审、谁受影响 |
| 范围边界 | 做什么、不做什么、暂缓什么 |
| 用户路径 | 用户如何开始、确认、推进、完成、查看结果、处理失败 |
| 验收口径 | 什么结果算产品侧通过 |

可选但有价值：

- 产品定位。
- 核心价值。
- 关键术语。
- 约束和依赖。
- 风险和开放问题。

## 4. PRD Gap 行为

如果缺少关键项：

- 先输出缺口，不要直接生成 Specs。
- 明确哪些内容会影响 Specs。
- 建议用户回到 PRD 澄清或 PRD 生成流程。
- 如果用户仍要求直接生成，使用 `assumption-based v0`，并把缺口写入 assumptions / pending / conflicts。

合格输出示例：

```md
## PRD Readiness Summary

- PRD source: `docs/v0.1-prd.md`
- Readiness: partial
- Confirmed: 版本目标、目标用户
- Missing: 验收口径、失败路径
- Conflicts: 无
- 建议：先补齐验收口径；否则只能生成 `assumption-based v0`。
```

## 5. PRD Readiness Summary

生成或修改 Specs 前，必须输出以下摘要，除非用户只是 Discussion / Review：

```md
## PRD Readiness Summary

- PRD source:
- Generation mode: PRD-first / assumption-based v0 / legacy version spec
- Version goal:
- Target users:
- Scope boundary:
- User flows:
- Acceptance:
- PRD gaps:
- Assumptions:
- Pending:
- Conflicts:
- Base extraction strategy:
```

如果某项未确认，写 `pending`，不要伪装成结论。

## 6. Base Extraction Strategy

从 PRD 派生 Specs 时：

- 当前版本目标、范围、路径和验收仍以 PRD 为准。
- Base Specs 只抽取长期复用规则。
- 如果某条内容只为当前版本成立，不写进 Base。
- 如果某条内容可能长期复用但尚未确认，标为 `base-candidate` 或 pending。
- 不把 PRD 中的一次性范围复制成项目级规则。

Base 准入测试：

```text
这条内容是否能被未来多个版本、多个 AI Coding 行动稳定复用？
如果不能，就不要进入 Base。
```

## 7. PRD Coverage Pass

PRD Coverage Pass 是 PRD-first 流程里的保真检查。它不要求把 PRD 复制成大矩阵，也不要求把版本需求写进 Base；它只要求重要 PRD 信息有明确去向。

### 7.1 目标

回答一个问题：

```text
PRD 中被识别为产品要求的内容，在 Specs 派生后去了哪里？
```

### 7.2 扫描范围

优先扫描高影响、高风险、用户可感知、影响验收的内容：

| 类型 | 示例 | 常见去向 |
|---|---|---|
| 版本目标和范围 | 本期要验证/交付什么 | 保留在 PRD；README 标明 PRD 权威 |
| 用户路径 | 首次使用、任务发起、确认、失败恢复 | `03-user-flows.md` 或保留在 PRD |
| 状态和状态变化 | 等待确认、执行中、失败、取消 | `04-states.md` |
| 文案和命名要求 | 提示语气、按钮命名、高风险确认 | `05-copy-guidelines.md` |
| 体验质量要求 | 顺滑、低干扰、易理解、适合手绘 | flow / copy / acceptance，或 coverage notes |
| 边界和异常 | 越权、目录越界、网络中断 | `06-edge-cases.md` |
| 验收口径 | 什么算通过/不通过 | `07-acceptance-principles.md` 或保留在 PRD |
| 专业项 | API、部署、测试矩阵、安全审查 | `08-delivery-handoff.md` |

### 7.3 处置状态

Coverage 不追求逐字映射，只追踪关键 PRD 要点。每个要点使用一个处置状态：

- `extracted-to-base`：已抽取到某份 Base Spec。
- `kept-in-prd`：只属于当前版本，仍以 PRD 为权威，不复制进 Base。
- `pending`：信息不足，不能写成结论。
- `conflict`：材料冲突，需要回到 PRD 裁决。
- `excluded-with-reason`：不属于产品侧 Specs 或不适合进入 Base，并已说明原因；例如交给研发/测试/安全后续文档。

### 7.4 输出方式

不要默认生成独立 coverage 文件。优先采用轻量输出：

- 在生成前的 `PRD Readiness Summary` 后补一段 `PRD Coverage Notes`。
- 在 `SPECS/README.md` 中写简短 coverage notes，记录重要的 `kept-in-prd`、`pending`、`conflict` 和 `excluded-with-reason`。
- 在最终交付报告中说明哪些 PRD 内容进入了哪些 Specs，哪些刻意保留在 PRD。

合格示例：

```md
## PRD Coverage Notes

| PRD 要点 | 处置状态 | 去向 / 原因 |
|---|---|---|
| 小屏画册需要手绘友好、滑动顺滑 | extracted-to-base | `03-user-flows.md` 浏览路径原则；`07-acceptance-principles.md` 体验验收 |
| 大屏 icon 不带文字 | pending | 可能是版本体验要求，也可能是长期文案/视觉原则；需产品确认后决定是否进入 Base |
| 自动化测试矩阵 | excluded-with-reason | 产品侧 Specs 不承载测试矩阵，交给测试文档 |
```

### 7.5 No Silent Drop

禁止信息静默丢失：

- 不能因为某条内容“不适合进 Base”就不再提及。
- 不能把 PRD 中的体验要求、边界要求或验收要求只留在 agent 记忆里。
- 不能用“Base 严格准入”解释所有遗漏；严格准入只决定是否进入 Base，不决定是否需要说明去向。
- 如果 PRD 内容很多，优先覆盖高影响、高风险、用户可感知、影响验收的内容。

## 8. 结构化确认

当 PRD 有缺口但用户希望继续推进时，可以用一轮结构化确认聚焦一个高影响问题。

规则：

- 一轮只问一个会改变 Specs 的问题。
- 提供 2-3 个互斥选项和推荐项。
- 问题服务于 readiness，而不是询问是否写文件。
- 用户说“不知道”时，给推荐默认值和取舍理由。

示例：

```md
我发现 PRD 中没有明确验收口径，这会影响 `07-acceptance-principles.md`。

A. 先不生成 Specs，回到 PRD 补齐验收
B. 生成 assumption-based v0，把验收口径标为 pending
C. 只生成 README 和定义类 Specs，暂不生成验收相关文档

推荐：A。因为验收口径会直接影响后续 AI Coding 判断“是否完成”。
```

## 9. 可跳过提问的条件

只有同时满足以下条件，才可以不提问：

- PRD 的最低 Ready 条件已经清楚、一致、无冲突。
- 用户明确说“直接生成 / 按你的判断来 / 不要追问”。

跳过提问时必须声明：

- 采用 `assumption-based v0` 或 PRD-first direct generation。
- 哪些是 assumptions。
- 哪些是 pending。
- 哪些 conflicts 被保留而非裁决。

用户只说“生成 specs”，不等于授权跳过 PRD Readiness Check。

## 10. 进入生成的条件

满足以下条件后可以生成或修改 Specs：

- 用户明确授权创建或修改文件。
- 当前不是 Discussion 或纯 Review 模式。
- Language Lock 已完成。
- PRD Readiness Check 已完成，或用户明确授权 `assumption-based v0`。
- PRD Readiness Summary 已形成。
- PRD Coverage Pass 已完成，或在 `assumption-based v0` 中明确 coverage assumptions / pending。
- 已标出 assumptions、pending、conflicts。
- 已明确 Base 只抽取长期复用规则。
- 当前输出模式明确。
