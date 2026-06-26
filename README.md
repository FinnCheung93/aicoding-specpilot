<div align="center">

## AI Coding 规范文档

基于 PRD 生成更适合 AI Coding 读取的产品规格文档。  
让需求从“产品表达”进一步变成“AI 可执行上下文”。

<sub>PRD · Spec · Structure · AI Coding</sub>

<br>

<p><kbd>Codex Skill</kbd> <kbd>Version v1.2.0</kbd> <kbd>Language 中文</kbd> <kbd>Series PM Workflow</kbd></p>

</div>

```text
AI Coding 规范文档用于把已经清楚的 PRD 转成 SDD-ready Specs。
它不替代 PRD，而是把版本需求整理成当前版本可执行的 Version Specs，以及长期可复用的 Base Specs。
```

## 🧭 PM 工作流系列

这一组 Skills 用来把产品想法逐步推进到 AI Coding 可执行状态。推荐顺序是：地基 → PRD → Spec → 原型。

| 顺序 | Skill | 作用 |
|---|---|---|
| 1 | [Agentic 地基](https://github.com/FinnCheung93/agentic-foundation) | 项目开工前，初始化原则、状态、日志和协作治理 |
| 2 | [PM PRD 助手](https://github.com/FinnCheung93/pm-prd-copilot) | 澄清、撰写、修订和审查开发可落地的 PRD |
| 3 | [AI Coding 规范文档](https://github.com/FinnCheung93/aicoding-specpilot) | 基于 PRD 生成更适合 AI Coding 读取的规格文档 |
| 4 | [PM 原型助手](https://github.com/FinnCheung93/finn-protopilot) | 基于 PRD / Spec 生成产品走查原型和演示材料 |

<sub>也可以单独调用其中任意一个 Skill，不一定必须按完整链路使用。</sub>

## ✨ 它能帮你做什么

- 从清晰 PRD 中生成当前版本的 Version Specs
- 从版本需求中提炼长期稳定的 Base Specs
- 把产品需求拆成更适合 AI Coding 的阅读单元
- 保留产品侧语义，不扩写成技术架构
- 记录 PRD 覆盖去向、Base 候选和质量检查项
- 帮助后续实现、审查和原型工作复用同一份规格上下文

## 🧭 适用场景

- 已经有 PRD，需要生成 AI Coding 友好的规格文档
- 一个版本需求比较复杂，需要拆成更稳定的规格结构
- 希望后续实现时少依赖长篇 PRD 反复检索
- 希望把产品需求和工程实现之间的语义层补齐
- 需要为增量版本留下清楚的执行规格和 Base 候选

## 🧩 工作方式

这个 Skill 会先检查 PRD 是否足够清楚，再决定是否生成 Specs。

- 先做语言锁定和 PRD 就绪检查
- PRD 缺口明显时，先指出缺口而不是硬写
- PRD 足够清楚时，生成轻量 Version Specs + Base Specs
- 生成后做 PRD 覆盖检查和 Review-Act 质量检查
- 保持产品侧规格，不越界到架构、接口和测试矩阵

## 🚦 边界

- 不替代 PRD 撰写
- 不负责工程架构、API 合同或测试用例设计
- 不在 PRD 不清楚时编造需求
- 不把规格文档写成过重的实现计划

## 📦 使用方式

把本目录作为 Codex skill 安装或放入你的 skills 目录中，由 Codex 在 PRD 到 Specs 的任务中自动触发。

```text
SKILL.md
```

<sub>未提供开源许可证，默认保留所有权利。</sub>
