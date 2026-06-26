---
name: aicoding-specpilot
description: >-
  Create PRD-first product-side Specs for AI Coding and Spec-Driven Development.
  Use when the user has a clear PRD or equivalent version requirements and wants
  to derive Version Specs plus stable Base Specs that AI Coding agents can read
  incrementally. Before generating or rewriting specs, run Language Lock, PRD
  Readiness Check, PRD Coverage Pass, and a Review-Act quality loop unless the
  user explicitly asks for assumption-based output. If no clear PRD is available,
  report PRD gaps and recommend a PRD clarification flow instead of fabricating
  requirements. Do not use as a replacement for PRD writing, architecture, API,
  test, security, or implementation-planning docs. If the user is only discussing
  whether specs/PRDs are needed, provide analysis and do not write files.
metadata:
  short-description: Build PRD-first SDD-ready Specs
  version: v1.2.0
  updated: 2026-06-24
---

# AI Coding SpecPilot

这个 skill 用于把明确的 PRD 或等价版本需求，转换成适合 AI Coding 渐进式读取的产品侧 Specs。它不是 PRD writer，也不是工程任务拆解器；默认前提是版本需求已经由 PRD 承载。

核心策略：**PRD-first，Version-first，Base 严格准入**。

- **PRD**：版本需求权威来源，承载为什么做、为谁做、本版本要达成什么。
- **Version Specs**：PRD-to-SDD 主产物，承载本版本如何被正确实现、验收和审查。
- **Base Specs**：项目级、长期可复用的产品侧规范，只收跨版本、跨 AI Coding 行动稳定复用的规则。
- **Engineering Specs**：架构、API、数据模型、测试矩阵、安全策略、实现计划等后续专业文档；本 skill 只做 handoff 建议，不默认生成。

## 0. 工作流总览

主流程：

```text
Intake Gate
-> PRD Source Detection
-> Language Lock
-> PRD Readiness Check
-> PRD Coverage Pass
-> Output Mode Selection
-> Write Authorization
-> Specs Generation
-> Review-Act Quality Loop
-> Delivery Report
```

如果没有明确 PRD 或等价版本需求输入，默认不直接生成 Specs。先输出 PRD 缺口分析，并建议用户进入 PRD 澄清或 PRD 生成流程。只有用户明确说“没有 PRD 也直接生成 / 按你的判断来 / assumption-based v0”，才可继续生成，并必须标注 assumptions / pending / conflicts。

用户只是开放讨论时，不要创建文件。

### 0.1 Intake Gate

开始前先判断：

- 目标项目或目标目录是否明确。
- 用户是想创建 Specs、优化已有 Specs、从 PRD 派生 SDD-ready Specs、只评审，还是只讨论规范体系。
- 是否存在 PRD、版本需求文档、Pre Docs、会议记录或其它输入资料。
- 用户是否明确授权写文件；如果没有，只输出分析、建议或待确认问题。
- 是否涉及 architecture、API、testing、security、implementation plan 等非产品侧文档；涉及时只在 handoff 中建议后续补充。

### 0.2 模式路由

| 模式 | 触发 | 行为 |
|---|---|---|
| Discussion | 用户问“怎么看”“是否需要 PRD”“PRD 和 Specs 怎么衔接” | 只分析和建议，不写文件 |
| PRD Readiness Review | 用户提供 PRD，希望判断能否生成 Specs | 检查 PRD 是否具备生成 Specs 的最低条件 |
| Create SDD Specs | 用户明确要求基于 PRD 生成进入 AI Coding 的 Specs | 默认生成 `SPECS/base/` + `SPECS/versions/<version>/` |
| Base-only | 用户只想沉淀长期项目规则 | 只生成或更新 `SPECS/base/`，并说明版本需求仍以 PRD 为准 |
| Refine existing | 用户明确要求优化或重组已有 Specs | 先说明将改哪些文件，再修改 |
| Assumption-based v0 | 用户明确要求没有 PRD 也直接生成 | 标注 assumptions / pending / conflicts 后生成 |
| Advanced Change Log | 用户明确要求文件化追踪 spec changes | 可生成 `spec-changes/`，但这是非默认路径 |

模式不清时，先问一个高影响确认问题，不要同时做多种模式。

### 0.3 Existing Specs Router

如果目标目录已有 `SPECS/`、旧 PRD、`versions/` 或 `spec-changes/`，先路由，不要直接新建或覆盖：

| 情况 | 默认行为 |
|---|---|
| 用户只想了解现状 | Review existing，只检查不修改 |
| 用户要优化当前体系 | Refine existing，在原体系上修改 |
| 用户有明确 PRD，要进入 AI Coding | Create SDD Specs，默认生成或更新当前版本 Specs 与必要 Base |
| 用户只要项目级规则 | Base-only |
| 用户明确要求文件化演进记录 | Advanced Change Log，可新增或更新 `spec-changes/` |
| 用户明确要求重建 | Rebuild，先说明旧内容如何保留 |

不要静默覆盖已有 Specs。需要重命名、迁移或重建时，先说明影响和保留策略。

## 1. PRD Readiness Check

`aicoding-specpilot` 的前置检查是 **PRD Readiness Check**。它检查 PRD 是否足够支撑 Version Specs 派生，而不是替代 PRD 做完整需求澄清。

最低检查面：

- 版本目标：本版本要达成、验证或交付什么。
- 目标用户：谁使用、谁评审、谁受影响。
- 范围边界：做什么、不做什么、暂缓什么。
- 用户路径：用户如何开始、推进、确认、完成、失败恢复。
- 验收口径：什么结果算产品侧通过。

如果任一项缺失、冲突或只是推断，优先报告 PRD gap，并建议回到 PRD 流程补齐。除非用户明确要求 `assumption-based v0`，不要在 Specs 里伪造版本需求。

详见 [references/requirement-clarification.md](references/requirement-clarification.md)。

## 2. PRD Coverage Pass

生成 Specs 前必须做轻量 **PRD Coverage Pass**：

- 识别 PRD 中关键需求、约束、体验原则、用户路径、状态、文案规则、边界情况和验收口径。
- 判断每类内容的去向：进入 Version Specs、进入 Base Specs、保留在 PRD、标为 pending / conflict，或交给后续专业文档。
- 不要求生成完整追踪矩阵，但必须能解释重要 PRD 信息为什么进入、暂不进入或不能进入 Specs。
- PRD 中被识别为产品要求的内容不得无解释消失。

判断测试：

```text
如果用户追问“PRD 里的这条要求去哪了？”，Version Specs、Base Specs 或交付报告应能回答。
```

## 3. Base 严格准入

Base Specs 只写长期复用内容。

进入 Base 的内容必须满足：

- 跨版本稳定复用。
- 跨 AI Coding 行动稳定复用。
- 不依赖某一次版本取舍才成立。
- 有助于 Agent 在后续实现、修改、评审时减少误读。

如果某条内容只属于当前版本，把它写入 Version Specs 或保留在 PRD。若不确定是否长期复用，写入当前版本的 `base-candidates.md`，不要直接提升为 Base。

## 4. 语言规则

默认使用用户或目标项目的主语言生成正文。当前用户使用中文时，Specs 正文必须自然使用中文；文件名、目录名和少量稳定术语保留英文。

- **Language Lock 是 preflight 的第一步**。在生成或重写文件前先锁定输出语言，不允许最后发现语言不对再整体翻译。
- 中文语境下，标题、正文、表格说明、验收标准、handoff 说明和总结都使用中文。
- 保留必要英文术语，例如 `AI Coding`、`SDD`、`PRD`、`Base Specs`、`Version Specs`、`Review-Act`、`Runtime`、`Agent`、`Artifact`、`API`、`CLI`、`Web`、`Desktop`、状态 ID、事件名和文件路径。
- 如果后期发现局部语言不一致，只局部修正受影响文字，不因为语言修正大范围重写结构、决策或无关内容。

## 5. 默认输出结构

除非用户指定其它位置，PRD-to-AI-Coding 默认在目标项目根目录生成：

```text
SPECS/
  README.md
  base/
    product-definition.md
    requirements-and-boundaries.md
    user-flows.md
    states.md
    copy-guidelines.md
    edge-cases.md
    acceptance-principles.md
    delivery-handoff.md
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

这是默认检查面，不是机械文件数量要求。小版本可以合并文件，但必须覆盖同等问题；没有结论的部分标为 pending 或 not-applicable，不要硬填模板。

默认不生成：

- `spec-changes/`
- architecture / API / test / security / implementation 文档

如果用户只要求长期项目规则，可走 Base-only 模式，只生成 `SPECS/base/` 和入口 README。

## 6. README 必备内容

`SPECS/README.md` 必须包含：

### PRD Source & Authority

- PRD 来源路径、文件名或材料说明。
- PRD 与 Specs 的权威关系：PRD 是版本需求权威，Specs 是派生规范。
- 冲突处理：如果 Specs 与 PRD 冲突，优先回到 PRD 裁决；若 PRD 已更新，则同步修正 Specs。
- 分层规则：版本需求进入 `versions/<version>/`；长期稳定规则进入 `base/`。

### Spec Change Rules

- Agent 不得静默修改 Base Specs。
- 发现 Base 缺口时，先提出 `base-candidate`。
- 用户或产品 owner 确认后，才可 Promote 到 Base。
- 不确定内容先标为 `pending`、`assumption`、`conflict` 或 `base-candidate`。

## 7. Review-Act Quality Loop

生成 Version Specs 后，必须做一次轻量 Review-Act 审查：

```text
Draft Version Specs
-> Self Coverage Check
-> PRD-vs-Spec Diff
-> Issue Classification P0/P1/P2/P3
-> Fix P0/P1
-> Record Review Notes
-> Re-check
-> P0/P1 = 0
```

P0/P1 清零后才可标记为 ready for AI Coding。P2/P3 可以记录为后续优化，不要求无限打磨。

Review notes 至少记录：PRD-vs-Spec diff 摘要、P0/P1/P2/P3 列表、P0/P1 处理结果。优先写入 `versions/<version>/README.md` 或最终交付报告；不要为了记录审查结果额外创建复杂目录。

如果环境允许且任务风险较高，可使用独立 subagent 审阅；审阅者必须同时看 PRD、Version Specs、Base Specs、coverage notes 和当前规则，不能只看最终文档。高风险包括：PRD 很长、已有 Specs 迁移、用户要求 ready for AI Coding、或 coverage / Base promotion 影响较大。

## 8. Reference Loading

不要默认读取全部 references。按任务加载：

- PRD readiness、PRD gaps、PRD Coverage Pass、是否可生成 -> [references/requirement-clarification.md](references/requirement-clarification.md)
- Base Specs 结构与 Base-only -> [references/base-spec-outline.md](references/base-spec-outline.md)
- Version Specs 结构 -> [references/version-spec-template.md](references/version-spec-template.md)
- Optional advanced change log -> [references/spec-changes-template.md](references/spec-changes-template.md)
- 最终质量检查和 Review-Act -> [references/spec-quality-gates.md](references/spec-quality-gates.md)

任务跨多个区域时，先处理最上游瓶颈，再按需加载下游 reference。

## 9. 生成规则

- 只有用户明确要求创建、重组、更新、生成或“应用”时，才写入文件。
- 如果用户只是问“怎么看”“是否合理”“应该怎么规划”，只输出分析和建议。
- 写文件前先完成 PRD Source Detection、Language Lock、PRD Readiness Check 和 PRD Coverage Pass。
- 如需写文件，再确认 Write Authorization。写文件授权不能替代 PRD Readiness Check。
- 如果只有 PRD 之外的模糊材料，先做 PRD gap 分析，不要直接改写为权威 Specs。
- 不静默覆盖已有 Specs。
- 不机械填模板。没有结论的章节可以标为 pending、not-applicable，或在 `assumption-based v0` 中明确假设。
- 默认不生成工程侧专业文档。
- PRD 中的关键产品要求不能因为 Base 严格准入而静默消失；不进入 Base 的内容也要说明进入 Version Specs、保留在 PRD、标为 pending，或交给后续专业文档。

## 10. 完成检查

结束前检查：

- 用户是否明确授权写文件；如果只是讨论，是否只输出了建议。
- 已完成 Language Lock，且没有依赖最后整体翻译修复语言。
- 已识别 PRD Source，并在 README 中写明权威关系。
- 已完成 PRD Readiness Summary，或已声明 `assumption-based v0` 并标注 assumptions / pending / conflicts。
- PRD Readiness Summary 覆盖版本目标、目标用户、范围边界、用户路径、验收口径。
- 已完成 PRD Coverage Pass，重要 PRD 信息没有无解释丢失。
- Version Specs 能回答本版本目标、范围、路径、状态、边界、验收和 handoff。
- Base Specs 没有复制 PRD 的一次性版本需求。
- Base candidates 没有被静默 Promote 到 Base。
- Review-Act Quality Loop 已完成，P0/P1 为 0 或已明确无法继续的阻塞。
- Handoff 只建议后续专业文档，不替代研发、测试、安全文档。
- 生成或修改 Specs 后，按 [references/spec-quality-gates.md](references/spec-quality-gates.md) 做质量检查。
