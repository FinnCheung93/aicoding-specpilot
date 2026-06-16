---
name: aicoding-specpilot
description: >-
  Create lightweight PRD-first product-side Specs for AI Coding projects. Use when the user has a
  clear PRD or equivalent version requirements and wants to derive Base Specs that are easier for
  AI Coding agents to read incrementally. Before generating or rewriting specs, run Language Lock
  and a PRD Readiness Check unless the user explicitly asks to proceed directly. If no clear PRD is
  available, report PRD gaps and recommend a PRD clarification flow instead of fabricating
  requirements. Do not use as a replacement for PRD writing, architecture, API, test, security, or
  implementation-planning docs. If the user is only discussing whether specs/PRDs are needed,
  provide analysis and do not write files.
metadata:
  short-description: Build PRD-first AI-coding-ready Specs
  version: v1.1.0
  updated: 2026-06-16
---

# AI Coding SpecPilot

这个 skill 用于把明确的 PRD 或等价版本需求，转换成适合 AI Coding 渐进式读取的产品侧 Specs。它不是 PRD writer，也不是工程任务拆解器；默认前提是版本需求已经由 PRD 承载，Specs 负责把 PRD 中可稳定复用的产品规则、路径、状态、文案、边界和验收口径抽象出来。

核心策略：**PRD-first，轻量 Specs，Base 严格准入**。

- **PRD**：版本需求权威来源，承载本版本目标、范围、路径、验收、非目标和待决问题。
- **Base Specs**：项目级、长期可复用的产品侧规范，只收跨版本、跨 AI Coding 行动稳定复用的规则。
- **Version Spec**：legacy / optional。只有在没有独立 PRD，或用户明确要求把版本 PRD 放进 Specs 内时才生成。
- **Spec Change Rules**：默认写在 `SPECS/README.md` 里。文件化 `spec-changes/` 属于 optional advanced flow，不默认生成。

## 0. 工作流总览

主流程：

```text
Intake Gate
-> PRD Source Detection
-> Language Lock
-> PRD Readiness Check
-> PRD Readiness Summary
-> PRD Coverage Pass
-> Write Authorization
-> Specs Generation
-> Quality Gates
-> 报告变更、PRD gaps 和后续建议
```

如果没有明确 PRD 或等价版本需求输入，默认不直接生成 Specs。先输出 PRD 缺口分析，并建议用户进入 PRD 澄清或 PRD 生成流程。只有用户明确说“没有 PRD 也直接生成 / 按你的判断来 / assumption-based v0”，才可继续生成，并必须标注 assumptions / pending / conflicts。

### 0.1 Intake Gate

开始前先判断：

- 目标项目或目标目录是否明确。
- 用户是想创建 Specs、优化已有 Specs、从 PRD 派生 Base Specs、只评审，还是只讨论规范体系。
- 是否存在 PRD、版本需求文档、Pre Docs、会议记录或其它输入资料。
- 用户是否明确授权写文件；如果没有，只输出分析、建议或待确认问题。
- 是否涉及 architecture、API、testing、security、implementation plan 等非产品侧文档；涉及时只在 handoff 中建议后续补充。

用户只是开放讨论时，不要创建文件。

### 0.2 模式路由

| 模式 | 触发 | 行为 |
|---|---|---|
| Discussion | 用户问“怎么看”“是否需要 PRD”“PRD 和 Specs 怎么衔接” | 只分析和建议，不写文件 |
| PRD Readiness Review | 用户提供 PRD，希望判断能否生成 Specs | 检查 PRD 是否具备生成 Specs 的最低条件 |
| Create from PRD | 用户明确要求基于 PRD 创建 Specs | 完成 PRD Readiness Check 后生成轻量 `SPECS/` |
| Refine existing | 用户明确要求优化或重组已有 Specs | 先说明将改哪些文件，再修改 |
| Assumption-based v0 | 用户明确要求没有 PRD 也直接生成 | 标注 assumptions / pending / conflicts 后生成 |
| Legacy Version Spec | 用户明确要求在 Specs 内承载版本 PRD | 可生成 `versions/<version>.md`，但这是非默认路径 |
| Advanced Change Log | 用户明确要求文件化追踪 spec changes | 可生成 `spec-changes/`，但这是非默认路径 |

模式不清时，先问一个高影响确认问题，不要同时做多种模式。

### 0.3 Existing Specs Router

如果目标目录已有 `SPECS/`、旧 PRD、`versions/<version>.md` 或 `spec-changes/`，先路由，不要直接新建或覆盖：

| 情况 | 默认行为 |
|---|---|
| 用户只想了解现状 | Review existing，只检查不修改 |
| 用户要优化当前体系 | Refine existing，在原体系上修改 |
| 用户有明确 PRD，要生成新 Specs | Create from PRD，默认只生成轻量 Base Specs |
| 用户明确要求新增版本文件 | Legacy Version Spec，可新增或更新 `versions/<version>.md` |
| 用户明确要求文件化演进记录 | Advanced Change Log，可新增或更新 `spec-changes/` |
| 用户明确要求重建 | Rebuild，先说明旧内容如何保留 |

用户没有明确要求新增版本文件时，不默认创建 `versions/`。

## 1. 适用场景

适用：

- 已有 PRD 或等价版本需求，准备进入 AI Coding。
- PRD 已说明版本目标、范围、路径和验收，但需要拆成 AI Coding 更容易按任务读取的产品侧 Specs。
- 长期迭代项目需要区分“版本需求权威”和“跨版本产品规则”。
- 产品侧需要向研发、架构、测试、安全或 AI Coding Agent 交付规范依据。

不适用：

- 用户需要从零澄清并撰写完整 PRD。
- 目标是直接生成 architecture、API spec、runtime contract、test matrix 或 security review。
- 用户已经明确要求进入工程实现，而不是整理产品侧规范。

## 2. PRD Readiness Check

`aicoding-specpilot` 的前置检查是 **PRD Readiness Check**。它检查 PRD 是否足够支撑 Specs 派生，而不是替代 PRD 做完整需求澄清。

最低检查面：

- 版本目标：本版本要达成、验证或交付什么。
- 目标用户：谁使用、谁评审、谁受影响。
- 范围边界：做什么、不做什么、暂缓什么。
- 用户路径：用户如何开始、推进、确认、完成、失败恢复。
- 验收口径：什么结果算产品侧通过。

如果任一项缺失、冲突或只是推断，优先报告 PRD gap，并建议回到 PRD 流程补齐。除非用户明确要求 `assumption-based v0`，不要在 Specs 里伪造版本需求。

详见 [references/requirement-clarification.md](references/requirement-clarification.md)。

## 3. Base 严格准入

Base Specs 只写长期复用内容。

进入 Base 的内容必须满足：

- 跨版本稳定复用。
- 跨 AI Coding 行动稳定复用。
- 不依赖某一次版本取舍才成立。
- 有助于 Agent 在后续实现、修改、评审时减少误读。

如果某条内容只属于当前版本，把它留在 PRD。若不确定是否长期复用，可在 `SPECS/README.md` 标记为 `base-candidate` 或 pending，不要直接提升为 Base。

判断测试：

```text
如果一条内容不能被多个版本、多个 AI Coding 行动稳定复用，就不要进入 Base。
```

## 3.1 PRD Coverage Pass

Base 严格准入不能导致 PRD 信息静默丢失。生成 Specs 前，必须做一次轻量 **PRD Coverage Pass**：

- 识别 PRD 中的关键需求、约束、体验原则、用户路径、状态、文案规则、边界情况和验收口径。
- 判断每类内容的去向：进入某份 Base Spec、保留在 PRD、标为 pending（可注明 `base-candidate`），或交给后续专业文档。
- 不要求生成完整追踪矩阵，但必须能解释重要 PRD 信息为什么进入、暂不进入或不能进入 Base。
- PRD 中被识别为产品要求的内容不得无解释消失。即使不进入 Base，也要在 `SPECS/README.md` 的 coverage notes 或最终交付报告中说明去向。

判断测试：

```text
如果用户追问“PRD 里的这条要求去哪了？”，Specs 或交付报告应能回答。
```

## 4. 语言规则

默认使用用户或目标项目的主语言生成正文。当前用户使用中文时，Specs 正文必须自然使用中文；文件名、目录名和少量稳定术语保留英文。

- **Language Lock 是 preflight 的第一步**。在生成或重写文件前先锁定输出语言，不允许最后发现语言不对再整体翻译。
- 中文语境下，标题、正文、表格说明、验收标准、handoff 说明和总结都使用中文。
- 保留必要英文术语，例如 `AI Coding`、`SDD`、`PRD`、`Base Specs`、`Version Spec`、`Spec Change Rules`、`Runtime`、`Daemon`、`Agent`、`Artifact`、`API`、`CLI`、`Web`、`Desktop`、状态 ID、事件名和文件路径。
- 如果后期发现局部语言不一致，只局部修正受影响文字，不因为语言修正大范围重写结构、决策或无关内容。

## 5. 默认输出结构

除非用户指定其它位置，默认在目标项目根目录生成：

```text
SPECS/
  README.md
  01-product-definition.md
  02-requirements-and-boundaries.md
  03-user-flows.md
  04-states.md
  05-copy-guidelines.md
  06-edge-cases.md
  07-acceptance-principles.md
  08-delivery-handoff.md
```

默认不生成：

- `versions/`
- `spec-changes/`
- architecture / API / test / security / implementation 文档

如果用户明确需要版本内置 PRD，可走 Legacy Version Spec。若用户明确需要文件化演进记录，可走 Advanced Change Log。

## 6. README 必备内容

`SPECS/README.md` 必须包含：

### PRD Source & Authority

- PRD 来源路径、文件名或材料说明。
- PRD 与 Specs 的权威关系：PRD 是版本需求权威，Specs 是派生规范。
- 冲突处理：如果 Specs 与 PRD 冲突，优先回到 PRD 裁决；若 PRD 已更新，则同步修正 Specs。
- 映射规则：PRD 中一次性版本需求不应被复制进 Base；只有稳定复用内容才进入 Base。

### Spec Change Rules

- Agent 不得静默修改 Base Specs。
- 发现 Specs 缺口时，先提出 change candidate。
- 用户或产品 owner 确认后，才可修改 Base。
- 不确定内容先标为 `pending`、`assumption` 或 `base-candidate`。

## 7. 生成规则

- 只有用户明确要求创建、重组、更新、生成或“应用”时，才写入文件。
- 如果用户只是问“怎么看”“是否合理”“应该怎么规划”，只输出分析和建议。
- 写文件前先完成 PRD Source Detection、Language Lock、PRD Readiness Check、PRD Readiness Summary 和 PRD Coverage Pass。
- 如需写文件，再确认 Write Authorization。写文件授权不能替代 PRD Readiness Check。
- 如果只有 PRD 之外的模糊材料，先做 PRD gap 分析，不要直接改写为权威 Specs。
- 不静默覆盖已有 Specs。
- 不机械填模板。没有结论的章节可以标为 pending、合理省略，或在 `assumption-based v0` 中明确假设。
- 默认不生成工程侧专业文档。
- `SPECS/README.md` 必须说明 PRD 权威关系、AI Coding 渐进式读取规则和 Spec Change Rules。
- PRD 中的关键产品要求不能因为 Base 严格准入而静默消失；不进入 Base 的内容也要说明保留在 PRD、标为 pending（可注明 `base-candidate`），或交给后续专业文档。

## 8. Reference Loading

不要默认读取全部 references。按任务加载：

- PRD readiness、PRD gaps、PRD Coverage Pass、是否可生成 -> [references/requirement-clarification.md](references/requirement-clarification.md)
- Base Specs 结构 -> [references/base-spec-outline.md](references/base-spec-outline.md)
- Legacy Version Spec -> [references/version-spec-template.md](references/version-spec-template.md)
- Optional advanced change log -> [references/spec-changes-template.md](references/spec-changes-template.md)
- 最终质量检查 -> [references/spec-quality-gates.md](references/spec-quality-gates.md)

任务跨多个区域时，先处理最上游瓶颈，再按需加载下游 reference。

## 9. Base Specs 内容

使用 [references/base-spec-outline.md](references/base-spec-outline.md) 生成 Base Specs。

必需 Base Specs：

- `README.md`：入口、PRD 来源与权威关系、阅读规则、状态、Spec Change Rules、后续文档建议。
- `01-product-definition.md`：产品是什么、不是什么、原则、角色、价值、长期边界。
- `02-requirements-and-boundaries.md`：长期业务目标、问题、范围边界、非目标、能力边界、成功原则。
- `03-user-flows.md`：可复用用户路径，不是版本承诺。
- `04-states.md`：产品可见状态词典。
- `05-copy-guidelines.md`：命名、语气、确认、错误、成功、安全提示等文案规则。
- `06-edge-cases.md`：用户可感知或产品必须裁决的边界情况。
- `07-acceptance-principles.md`：产品验收维度，不是完整测试矩阵。
- `08-delivery-handoff.md`：建议研发、测试、安全和 AI Coding 后续补充哪些文档。

## 10. Optional / Legacy 能力

### Legacy Version Spec

仅在以下情况使用 [references/version-spec-template.md](references/version-spec-template.md)：

- 用户没有独立 PRD，但明确要求在 Specs 内承载版本 PRD。
- 用户已有旧 `versions/` 体系，并明确要求继续维护。
- 用户明确要求生成 `versions/<version>.md`。

### Advanced Change Log

仅在以下情况使用 [references/spec-changes-template.md](references/spec-changes-template.md)：

- 项目进入长期多人协作，需要文件化追踪 change candidates。
- 用户明确要求创建 `spec-changes/`。
- 轻量 README 内规则不足以管理频繁的 Base 演进。

## 11. 完成检查

结束前检查：

- 用户是否明确授权写文件；如果只是讨论，是否只输出了建议。
- 已完成 Language Lock，且没有依赖最后整体翻译修复语言。
- 已识别 PRD Source，并在 README 中写明权威关系。
- 已完成 PRD Readiness Summary，或已声明 `assumption-based v0` 并标注 assumptions / pending / conflicts。
- PRD Readiness Summary 覆盖版本目标、目标用户、范围边界、用户路径、验收口径。
- 已完成 PRD Coverage Pass，重要 PRD 信息没有无解释丢失。
- 如果 PRD 缺关键项，是否先报告了 PRD gaps，而不是在 Specs 中伪造结论。
- Base Specs 没有复制 PRD 的一次性版本需求。
- 默认没有生成 `versions/` 或 `spec-changes/`。
- Handoff 只建议后续专业文档，不替代研发、测试、安全文档。
- 生成或修改 Specs 后，按 [references/spec-quality-gates.md](references/spec-quality-gates.md) 做质量检查。
- 如果已有 `SPECS/`，报告哪些文件被创建、修改或刻意保留。
