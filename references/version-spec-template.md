# Version Spec Template（Legacy / Optional）

`Version Spec` 是 legacy / optional 能力，不是 `aicoding-specpilot` 的默认输出。

默认情况下，版本目标、范围、路径和验收由 PRD 承载。只有在以下情况才使用本模板：

- 用户没有独立 PRD，但明确要求在 Specs 内承载版本 PRD。
- 用户已有旧 `versions/` 体系，并明确要求继续维护。
- 用户明确要求生成或更新 `versions/<version>.md`。

如果存在独立 PRD，PRD 仍是版本需求权威。Version Spec 不应与 PRD 产生双重权威。

## 1. 使用前检查

生成前先确认：

- 用户明确要求生成 Version Spec。
- 是否存在 PRD；如存在，写明 PRD authority。
- 该 Version Spec 是临时承载版本需求，还是旧体系维护。
- 是否会造成 PRD 与 Specs 双重维护。

## 2. 推荐文件名

```text
SPECS/
  versions/
    v0.1.md
```

文件名可以按用户指定版本调整。

## 3. 模板

```md
# Version Spec: <version>

## 1. Status

- State:
- Source:
- PRD authority:
- Last updated:

## 2. Version Goal

说明本版本要达成、验证或交付什么。

## 3. Target Users

说明本版本面向谁、谁评审、谁受影响。

## 4. Scope

### 4.1 In Scope

- ...

### 4.2 Out of Scope

- ...

### 4.3 Deferred

- ...

## 5. Selected Base Specs

说明本版本需要重点读取哪些 Base Specs。

## 6. User Flows

说明本版本实际启用或验证的用户路径。

## 7. States

说明本版本必须处理的状态。

## 8. Copy Requirements

说明本版本需要特别确认的文案要求。

## 9. Edge Cases

说明本版本必须覆盖的边界情况。

## 10. Product Acceptance

说明本版本产品侧验收口径。

## 11. Non-goals

说明本版本明确不做的事项。

## 12. Base Candidates

记录可能未来提升为 Base 的内容，但当前不直接修改 Base。

## 13. Open Decisions

- ...

## 14. Handoff Needs

说明研发、测试、安全或架构后续需要补充什么。
```

## 4. 注意事项

- 不复制 Base Specs 正文。
- 不把未经确认的一次性版本需求提升为 Base。
- 如果有 PRD，优先引用 PRD，不要创造第二套版本需求权威。
- 如果用户只是要基于 PRD 生成轻量 Base Specs，不要使用本模板。
