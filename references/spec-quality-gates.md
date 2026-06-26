# Spec Quality Gates

这个 reference 用于在生成或修改 Specs 后做质量检查。它不是测试矩阵，而是判断规格是否足够清晰、可评审、可被 AI Coding Agent 渐进式读取。

## 1. 必过项

以下项目不满足时，不应直接交付 Specs：

- **写文件授权明确**：用户明确要求创建、重组、更新、生成或应用；如果用户只是在讨论，应只输出建议。
- **模式没有混淆**：Discussion / PRD Readiness Review / Create SDD Specs / Base-only / Refine existing / Assumption-based v0 / Advanced Change Log 已明确，且行为与模式一致。
- **Language Lock 已完成**：正文语言在生成前已确定；中文用户的正文为中文，文件名和稳定术语可保留英文。
- **PRD Source 已识别**：README 中必须写明 PRD 来源或等价版本需求来源。
- **PRD Source & Authority 已写明**：PRD 是版本需求权威，Specs 是派生规范；冲突时回到 PRD 裁决或同步修正 Specs。
- **PRD Readiness Check 已完成**：生成前必须有 PRD Readiness Summary；或用户明确授权直接生成，并声明 `assumption-based v0`。
- **PRD Readiness Summary 覆盖最低面**：版本目标、目标用户、范围边界、用户路径、验收口径。
- **PRD Coverage Pass 已完成**：PRD 中的关键产品要求、体验原则、边界和验收口径有明确去向。
- **Version Specs 已承接版本执行**：当前版本目标、范围、路径、状态、边界、验收和 handoff 有明确位置，或标为 pending / not-applicable。
- **No Silent Drop**：不进入 Specs 的重要 PRD 信息也要说明保留在 PRD、标为 pending/conflict，或交给后续专业文档。
- **PRD gaps 未被伪造成结论**：缺失、冲突或推断内容必须标为 gap / pending / assumption / conflict。
- **Base 严格准入**：Base Specs 只收跨版本、跨 AI Coding 行动稳定复用的内容。
- **Base 未被静默 Promote**：`base-candidate` 只有经用户或产品 owner 确认后才能进入 Base。
- **Review-Act 已完成**：P0/P1 已修复、误判说明清楚，或明确存在无法继续的阻塞。
- **Review Notes 有落点**：PRD-vs-Spec diff 摘要、问题分级和 P0/P1 处理结果已写入 `versions/<version>/README.md`、最终交付报告，或用户指定位置。
- **没有越界文档**：产品侧 Specs 没有冒充 architecture、API spec、runtime contract、test matrix 或 security review。

## 2. Review-Act 分级

| 级别 | 定义 | 完成要求 |
|---|---|---|
| P0 | PRD 意图被反向、核心范围遗漏、会驱动错误实现 | 必须修复并复审 |
| P1 | 重要 PRD 要求缺失、验收不可判定、关键边界无处理、权威关系冲突 | 必须修复并复审 |
| P2 | 不阻断开发但影响清晰度、覆盖度、handoff 或维护成本 | 记录，能修则修 |
| P3 | 文案、格式、命名、小型可读性问题 | 可记录为后续优化 |

完成标准：P0/P1 清零；P2 有处理或暂缓说明；P3 不阻塞。

## 3. PRD-first / Version-first 质量

检查：

- 是否保留 PRD source，避免把输入材料洗成无来源结论。
- 是否说明 PRD、Version Specs、Base Specs 的权威关系。
- 是否检查 PRD 包含版本目标、目标用户、范围边界、用户路径、验收口径。
- 是否对 PRD 中的关键需求、体验要求、边界、状态、文案和验收口径做了 coverage 判断。
- 是否能回答“PRD 里的这条要求去哪了”。
- 如果 PRD 不完整，是否先报告 PRD gaps，而不是直接补造需求。
- 如果跳过提问，是否满足用户明确授权，并标注 assumptions / pending / conflicts。
- 是否避免把 PRD 中的一次性版本需求提升为 Base。
- 是否把可能长期复用但未确认的内容标为 `base-candidate` 或 pending。

如果 preflight 只包含判断和“是否继续 / 是否生成 / 是否按推荐执行”，但没有 PRD readiness 判断，判定为未完成前置检查。

## 4. Version Specs 检查

Version Specs 应回答当前版本“如何做对”。

检查：

- `README` 是否说明 version id、PRD source、状态和阅读顺序。
- `requirements` 是否明确目标、范围、非目标、暂缓项和约束。
- `flows` 是否覆盖本版本关键用户路径。
- `states` 是否覆盖用户可感知状态和转换。
- `acceptance` 是否可判定，可使用 Given / When / Then 或等价结构。
- `edge-cases` 是否覆盖用户可感知或产品必须裁决的边界。
- `coverage` 是否解释重要 PRD 要点去向。
- `base-candidates` 是否只记录候选，不静默修改 Base。
- `handoff` 是否说明工程侧还需补哪些 specs。

文件可以合并，但检查面不能无解释消失。

## 5. Base Specs 检查

Base Specs 应稳定、严格、可长期复用。

检查：

- 是否写入了过多某个 PRD 版本的临时范围。
- 是否把版本暂缓项写成长期非目标。
- 是否把不适合进入 Base 的 PRD 内容说明为 `version-spec`、`kept-in-prd`、`pending`、`conflict` 或 `handoff`，而不是直接遗漏。
- 是否让状态、文案、边界情况有统一词汇。
- 是否避免重复定义。
- 是否保留后续专业文档建议，但没有替代专业文档。
- 是否只收跨版本、跨 AI Coding 行动稳定复用的内容。

## 6. 可操作性

检查：

- AI Coding Agent 能否按任务只读 1-3 份相关文档。
- `SPECS/README.md` 是否说明阅读顺序和权威关系。
- 已有 `SPECS/` 时，是否优先更新现有体系，而不是无意中另起一套。
- 已有 PRD / `SPECS/` / legacy `versions/` / legacy `spec-changes/` 时，是否先经过 Existing Specs Router。
- Version Specs 是否足以进入后续 plan / tasks / implementation。
- Handoff 是否说明哪些内容不由产品侧 Specs 决定。

## 7. 语言质量

检查：

- 中文语境下标题、正文、表格说明、验收和 handoff 是否为中文。
- 英文是否主要保留在文件名、路径、稳定术语、状态 ID 或接口类名中。
- 是否存在“先英文生成，最后整体翻译”的痕迹。
- 是否因为语言修正导致结构和决策被大范围重写。

如果发现语言错误，只修正受影响文本，不重写无关结构。

## 8. Optional / Advanced Change Log 检查

如果用户明确要求生成 `spec-changes/`，才检查本节。

检查：

- 是否说明 Detect -> Propose -> Review -> Promote。
- 是否禁止 AI 静默修改 Base Specs。
- 是否提供 candidate 模板。
- 是否说明 Promote 后需要同步更新哪些文档。
- 是否没有把当前 PRD 的一次性需求错误提升为 Base。

## 9. 交付检查

交付前简短报告：

- 创建了哪些文件。
- 修改了哪些文件。
- 哪些文件刻意没有修改。
- 当前 Specs 状态：Draft / Review / Ready for AI Coding / Approved。
- PRD-vs-Spec diff 摘要。
- P0/P1 是否清零。
- P0/P1 的修复或误判说明。
- P2/P3 的保留或暂缓说明。
- 仍有哪些 PRD gaps、pending 或 conflicts。
- 下一步建议谁来评审。

如果用户只要求分析，不要写文件；如果用户要求修改，只修改目标 skill 或目标项目中明确范围内的文件。
