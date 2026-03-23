---
name: sdd-orchestrator
description: 用于编排 Spec-Driven Development（SDD）工作流，包括需求澄清、Specify、Plan、Tasks、Execution Contract、Implement 交接、Review、跨会话恢复与文档联动维护。当用户希望按 SDD 推进项目、起草或审核 spec/plan/tasks 文档、判断是否可进入下一阶段、恢复中断项目、准备 Coding Agent 交接，或在 MVP 后判断新增功能应走轻量还是完整 SDD 时使用。
---

# SDD Orchestrator

将本 skill 作为 **SDD v2 协作编排器** 使用。  
它负责阶段识别、阶段门禁、文档起草、状态维护、中断恢复与 Implement 前 handoff 编排。  
它不负责替代 Coding Agent 执行具体实现。

触发本 skill 后，先识别当前属于哪类场景：
- 新项目进入 SDD
- 起草某一阶段文档
- 判断能否进入下一阶段
- 恢复中断项目
- MVP 后新增功能分级
- Implement 前 handoff
- Review / 验收

先读最小必要文档集，不要一开始全读。

---

## 核心硬规则

1. **不越阶段**：未确认当前阶段前，不进入下一阶段。已写出不等于已确认。  
2. **草案不等于定稿**：阶段文档在用户明确确认前，只能视为草案。  
3. **默认先骨架、后正式稿**：仅在用户明确要求、已高度熟悉结构或边界极清时，才直接输出完整草案。  
4. **无真实痕迹不算已推进**：文档修订、代码改动、验证结果、任务状态、review 结论、阶段摘要，至少具备其一。  
5. **恢复优先看状态**：新对话或中断恢复时，优先读取 `sdd-status.md`。  
6. **文档联动更新必须及时**：核心文档状态或内容变化后，必须同步检查相关联文档，尤其是 `sdd-status.md`。  
7. **阶段确认后的状态回写不可遗漏**：当某阶段门禁通过、允许进入下一阶段时，必须先把当前阶段主文档头部的“当前状态”从“草案中/待审核”等更新为“已确认”，再同步更新 `sdd-status.md`、下一步动作与阶段切换结论；不得出现状态卡已进入下一阶段、主文档头仍停留在草案中的不一致状态。  
8. **阶段确认后的文档命名收口不可遗漏**：若项目采用文件名表达草案属性（如“草案”“初稿”等），当阶段门禁通过并确认进入下一阶段时，必须同步检查并更新文件名，使其与文档头状态一致；不得出现文件名仍带“草案/初稿”，但文档头已写“已确认”的不一致状态。若项目采用固定文件名，则至少确认无需改名并保持文档头状态一致。  
9. **阶段主文档未正式落盘，不得口头进入下一阶段**：对话中的草案、口头收口、消息内骨架都不能替代项目内的阶段主文档。若某一轮对应阶段主文档尚未正式落盘到项目中，则该阶段只能视为“讨论中”，不得宣告进入下游阶段，也不得起草或推进下游阶段文档。  
10. **严格区分文档状态与阶段状态**：不要混用。  
11. **功能级 SDD 默认写增量**：继承项目级背景与主结构决策，不重复抄写。

如需完整规则，读取：
- `references/SDD 模板总则.md`
- `references/SDD 联动更新矩阵.md`

---

## 默认阶段顺序

1. 需求讨论
2. Specify
3. Plan
4. Tasks
5. Execution Contract
6. Implement
7. Review / Acceptance

---

## 场景分流

### 1. 新项目进入 SDD
默认动作：
- 进入需求讨论
- 判断是否具备进入 Specify 的条件
- 先起草 `spec.md` 骨架
- 同步准备 `sdd-status.md`

读取：
- `references/core-templates/spec.md`
- `references/checklists/specify-checklist.md`
- `references/core-templates/sdd-status.md`

### 2. 起草某一阶段文档
默认动作：
- 检查前置阶段是否已确认
- 条件不足时先补缺
- 条件足够时按默认策略起草
- 将当前阶段主文档正式落盘到项目中（至少形成草案文件）
- 起草后检查联动更新

读取：
- 当前阶段主模板
- 当前阶段 checklist
- 必要时 `references/SDD 联动更新矩阵.md`

### 3. 判断能否进入下一阶段
默认动作：
- 先确认当前阶段主文档已正式落盘到项目中；若尚未落盘，先补文档，不做阶段推进判断
- 用当前阶段主文档 + checklist 做门禁检查
- 明确指出“已满足”或“还缺什么”
- 若推进，先回写当前阶段主文档头部状态为“已确认”，再同步更新 `sdd-status.md`、下一步动作与阶段切换结论

读取：
- 当前阶段主文档
- 当前阶段 checklist
- `references/core-templates/sdd-status.md`

### 4. 恢复中断项目
默认动作：
- 先看 `sdd-status.md`
- 再看当前阶段主文档与对应 checklist
- 若状态冲突，先修正文档自洽
- 从“下一步唯一推荐动作”继续

读取：
- `references/core-templates/sdd-status.md`
- 当前阶段主文档
- 对应 checklist
- 必要时 `references/SDD 联动更新矩阵.md`

### 5. MVP 后新增功能分级
默认动作：
- 先判断变更级别：微小 / 中等 / 大功能
- 再决定：轻量记录 / 轻量 SDD / 完整功能级 SDD
- 功能级 SDD 默认写增量

读取：
- `references/SDD 模板总则.md`
- `references/core-templates/spec.md`
- `references/core-templates/plan.md`
- `references/core-templates/tasks.md`

### 6. Implement 前 handoff
默认动作：
- 检查 `spec.md`、`plan.md`、`tasks.md` 是否已确认
- 检查 `execution-contract.md` 是否已形成可执行版本
- 用 handoff checklist 做最后校验
- 通过后更新 `sdd-status.md` 与下一步动作

读取：
- `references/core-templates/execution-contract.md`
- `references/checklists/implement-handoff-checklist.md`
- `references/core-templates/sdd-status.md`
- `references/Coding Agent Handoff 协议.md`
- `references/实现层入口与链路.md`

### 7. Review / 验收
默认动作：
- 优先对照 `spec.md`
- 再检查任务完成情况与验证结果
- 再形成结构化 review 结论
- 用 review checklist 做收口检查
- 结论形成后同步更新 `sdd-status.md`

读取：
- `references/core-templates/review.md`
- `references/checklists/review-checklist.md`
- `references/core-templates/spec.md`
- `references/core-templates/tasks.md`

---

## 读取导航

### 理解模板体系本身
- `references/索引与导航.md`
- `references/SDD 模板总则.md`
- `references/SDD 联动更新矩阵.md`

### 推进某一阶段
- 对应主模板
- 对应 checklist
- `references/core-templates/sdd-status.md`

### 处理联动与恢复
- `references/SDD 联动更新矩阵.md`
- `references/core-templates/sdd-status.md`

### 进入实现层或理解 handoff 链路
- `references/实现层入口与链路.md`
- `references/Coding Agent Handoff 协议.md`
- `references/core-templates/execution-contract.md`
- `references/checklists/implement-handoff-checklist.md`

### 理解 references 子目录
- `references/core-templates/模板说明.md`
- `references/checklists/清单说明.md`

---

## 目录入口

### 核心模板
目录：`references/core-templates/`
- `spec.md`
- `plan.md`
- `tasks.md`
- `execution-contract.md`
- `sdd-status.md`
- `review.md`

### 阶段检查清单
目录：`references/checklists/`
- `specify-checklist.md`
- `plan-checklist.md`
- `tasks-checklist.md`
- `implement-handoff-checklist.md`
- `review-checklist.md`

### 上位规则与配套协议
- `references/SDD 模板总则.md`
- `references/SDD 联动更新矩阵.md`
- `references/索引与导航.md`
- `references/Coding Agent Handoff 协议.md`
- `references/实现层入口与链路.md`

---

## 不要在本 skill 中做的事

本 skill 不负责：
- 具体语言/框架的编码细节规范
- 实现阶段的逐步施工指导
- 大量代码层面的 review 细则展开
- 子 agent 的具体命令执行编排
- 直接以“实现细节专家”身份替代 Coding Agent

这些内容应交由配套 skill 或 Coding Agent 处理。
