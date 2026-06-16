# Spec Quality Gates

这个 reference 用于在生成或修改 Specs 后做质量检查。它不是测试矩阵，而是判断规格是否足够清晰、可评审、可被 AI Coding Agent 渐进式读取。

## 1. 必过项

以下项目不满足时，不应直接交付 Specs：

- **写文件授权明确**：用户明确要求创建、重组、更新、生成或应用；如果用户只是在讨论，应只输出建议。
- **模式没有混淆**：Discussion / PRD Readiness Review / Create from PRD / Refine existing / Assumption-based v0 / Legacy Version Spec / Advanced Change Log 已明确，且行为与模式一致。
- **Language Lock 已完成**：正文语言在生成前已确定；中文用户的正文为中文，文件名和稳定术语可保留英文。
- **PRD Source 已识别**：README 中必须写明 PRD 来源或等价版本需求来源。
- **PRD Source & Authority 已写明**：PRD 是版本需求权威，Specs 是派生规范；冲突时回到 PRD 裁决或同步修正 Specs。
- **PRD Readiness Check 已完成**：生成前必须有 PRD Readiness Summary；或用户明确授权直接生成，并声明 `assumption-based v0`。
- **PRD Readiness Summary 覆盖最低面**：版本目标、目标用户、范围边界、用户路径、验收口径。
- **PRD Coverage Pass 已完成**：PRD 中的关键产品要求、体验原则、边界和验收口径有明确去向。
- **No Silent Drop**：不进入 Base 的重要 PRD 信息也要说明保留在 PRD、标为 pending/conflict，或交给后续专业文档。
- **PRD gaps 未被伪造成结论**：缺失、冲突或推断内容必须标为 gap / pending / assumption / conflict。
- **Base 严格准入**：Base Specs 只收跨版本、跨 AI Coding 行动稳定复用的内容。
- **没有重复版本 PRD**：默认不把 PRD 的一次性版本目标、范围、路径和验收复制进 Base。
- **默认轻量结构**：默认不生成 `versions/` 或 `spec-changes/`。
- **Spec Change Rules 已写入 README**：Agent 不得静默修改 Base；发现缺口先提出 change candidate。
- **没有越界文档**：产品侧 Specs 没有冒充 architecture、API spec、runtime contract、test matrix 或 security review。

## 2. 语言质量

检查：

- 中文语境下标题、正文、表格说明、验收和 handoff 是否为中文。
- 英文是否主要保留在文件名、路径、稳定术语、状态 ID 或接口类名中。
- 是否存在“先英文生成，最后整体翻译”的痕迹。
- 是否因为语言修正导致结构和决策被大范围重写。

如果发现语言错误，只修正受影响文本，不重写无关结构。

## 3. PRD-first 质量

检查：

- 是否保留 PRD source，避免把输入材料洗成无来源结论。
- 是否说明 PRD 与 Specs 的权威关系。
- 是否检查 PRD 包含版本目标、目标用户、范围边界、用户路径、验收口径。
- 是否对 PRD 中的关键需求、体验要求、边界、状态、文案和验收口径做了 coverage 判断。
- 是否能回答“PRD 里的这条要求去哪了”。
- 如果 PRD 不完整，是否先报告 PRD gaps，而不是直接补造需求。
- 如果跳过提问，是否满足用户明确授权，并标注 assumptions / pending / conflicts。
- 是否避免把 PRD 中的一次性版本需求提升为 Base。
- 是否把可能长期复用但未确认的内容标为 `base-candidate` 或 pending。

如果 preflight 只包含判断和“是否继续 / 是否生成 / 是否按推荐执行”，但没有 PRD readiness 判断，判定为未完成前置检查。

## 4. 决策价值

好的 Specs 应该帮团队减少歧义，而不是堆叠信息。

检查：

- 每份文档是否回答一个清晰问题。
- 每条规则是否能指导后续实现、评审或验收。
- 是否把推断写成了已确认事实。
- 是否把待定事项伪装成了结论。
- 是否保留了足够少但关键的 pending。

如果一个章节只有模板标题、没有决策价值，应删除、合并或标为 pending。

## 5. 可操作性

检查：

- AI Coding Agent 能否按任务只读 1-3 份相关文档。
- `README.md` 是否说明阅读顺序和权威关系。
- 已有 `SPECS/` 时，是否优先更新现有体系，而不是无意中另起一套。
- 已有 PRD / `SPECS/` / legacy `versions/` / legacy `spec-changes/` 时，是否先经过 Existing Specs Router。
- `03-user-flows.md` 是否能指导流程串联。
- `04-states.md` 是否能指导 UI / CLI / runtime 状态表达。
- `05-copy-guidelines.md` 是否能指导高风险提示和错误文案。
- `06-edge-cases.md` 是否能指导异常体验决策。
- `07-acceptance-principles.md` 是否能支持产品侧验收。

## 6. Base Specs 检查

Base Specs 应稳定、严格、可长期复用。

检查：

- 是否写入了过多某个 PRD 版本的临时范围。
- 是否把版本暂缓项写成长期非目标。
- 是否把不适合进入 Base 的 PRD 内容说明为 `kept-in-prd`、`pending`、`conflict` 或 `excluded-with-reason`，而不是直接遗漏。
- 是否让状态、文案、边界情况有统一词汇。
- 是否避免重复定义。
- 是否保留后续专业文档建议，但没有替代专业文档。
- 是否只收跨版本、跨 AI Coding 行动稳定复用的内容。

## 7. Optional / Legacy 检查

如果用户明确要求生成 `versions/` 或 `spec-changes/`，才检查本节。

### 7.1 Legacy Version Spec

检查：

- 是否明确这是 legacy / optional 路径。
- 如果有 PRD，是否写明 PRD 仍是版本需求权威。
- 是否避免与 PRD 产生双重权威。
- 是否没有静默修改 Base 规则。

### 7.2 Advanced Change Log

检查：

- 是否说明 Detect -> Propose -> Review -> Promote。
- 是否禁止 AI 静默修改 Base Specs。
- 是否提供 candidate 模板。
- 是否说明 Promote 后需要同步更新哪些文档。
- 是否没有把当前 PRD 的一次性需求错误提升为 Base。

## 8. 交付检查

交付前简短报告：

- 创建了哪些文件。
- 修改了哪些文件。
- 哪些文件刻意没有修改。
- 当前 Specs 状态：Draft / Review / Approved。
- 仍有哪些 PRD gaps、pending 或 conflicts。
- 下一步建议谁来评审。

如果用户只要求分析，不要写文件；如果用户要求修改，只修改目标 skill 或目标项目中明确范围内的文件。
