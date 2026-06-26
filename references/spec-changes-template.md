# Spec Changes Template（Optional Advanced Flow）

`spec-changes/` 是 optional advanced flow，不是默认输出。

默认情况下，Base 演进先由 `SPECS/versions/<version>/base-candidates.md` 承载，轻量 Spec Change Rules 写在 `SPECS/README.md` 中即可：

- Agent 不得静默修改 Base Specs。
- 发现缺口先提出 change candidate。
- 用户或产品 owner 确认后，才可修改 Base。
- 不确定内容先标为 `pending`、`assumption` 或 `base-candidate`。

只有当项目进入长期多人协作、频繁发生 Base 演进，或用户明确要求文件化追踪时，才创建 `spec-changes/`。

## 1. 推荐目录

```text
SPECS/
  spec-changes/
    README.md
    candidates/
      YYYY-MM-DD-short-title.md
```

## 2. README.md 建议内容

```md
# Spec Changes

## 1. Purpose

本目录用于记录可能需要修改 Base Specs 的 change candidates。

## 2. Flow

Detect -> Propose -> Review -> Promote

## 3. Rules

- Agent 不得静默修改 Base Specs。
- 每个 candidate 必须说明来源、影响和推荐处理；来源可以是某个 Version Spec 的 `base-candidates.md`。
- 用户或产品 owner 确认后，才可 Promote。
- 被拒绝或暂缓的 candidate 必须保留状态。

## 4. Candidate Status

- proposed
- accepted
- rejected
- deferred
- promoted

## 5. Candidate Template

见 `candidates/YYYY-MM-DD-short-title.md`。
```

## 3. Candidate 模板

```md
# Spec Change Candidate: <title>

## 1. Status

- State: proposed
- Source:
- Created:
- Owner:

## 2. Trigger

说明为什么出现这个 candidate。

## 3. Current Spec Gap

说明当前 Specs 中缺什么、冲突什么或过时什么。

## 4. Evidence

说明证据来自 PRD、实现、评审、用户反馈还是多次重复情况。

## 5. Recommended Change

说明建议修改哪些 Base Specs。

## 6. Impact

说明对产品定义、路径、状态、文案、边界或验收的影响。

## 7. Risks

说明错误提升为 Base 的风险。

## 8. Decision

- accepted / rejected / deferred
- decision maker:
- date:
- notes:

## 9. Promotion Notes

如果 accepted，说明已修改哪些 Specs。
```

## 4. 提升规则

只有同时满足以下条件，candidate 才能 Promote：

- 影响跨版本或跨 AI Coding 行动。
- 不是某个 PRD 的一次性需求。
- 用户或产品 owner 明确确认。
- 已说明需要修改的具体 Base Specs。
- 已记录对 PRD 映射或 README authority 说明的影响。

## 5. 不应创建 candidate 的情况

- 只是某个版本 PRD 的新增范围。
- 已能在当前 Version Spec 的 `base-candidates.md` 轻量记录。
- 只是实现细节、API 细节、测试用例或安全策略细节。
- 用户尚未确认是否需要长期复用。
- 可以直接在当前 PRD 中解决。
