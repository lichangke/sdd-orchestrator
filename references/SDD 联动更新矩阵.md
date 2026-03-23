# 《SDD 联动更新矩阵（初稿）》

## 0. 文档目的

本矩阵用于把《SDD 模板总则（初稿）》中的“文档联动更新规则”进一步具体化。  
它回答的是：

- 哪个文档变更后，要检查哪些关联文档
- 哪些变更只是状态同步，哪些会触发内容级联动
- 在实现推进、阶段切换、review 收口时，最低应同步哪些文档

本矩阵作为 SDD 文档维护、恢复推进、skill 自动起草与执行编排时的联动依据。

---

## 1. 联动更新的基本原则

### 1.1 两类更新
文档更新默认分为两类：

#### A. 状态更新
指文档内容主体不变，但状态发生变化，例如：
- 草案中 → 待审核
- 待审核 → 已确认
- 已确认 → 已替换

#### B. 内容更新
指文档中的核心内容发生变化，例如：
- 目标变化
- 边界变化
- FR / NFR 变化
- 方案路径变化
- 任务依赖变化
- review 结论变化

---

### 1.2 联动强度
#### 低强度联动
适用于状态更新。  
重点是同步：
- 文档头状态
- `sdd-status.md`

#### 高强度联动
适用于内容更新。  
重点是：
- 同步状态
- 检查上下游文档是否仍然有效
- 必要时要求重新审核或回退阶段

---

## 2. 核心文档联动矩阵

| 变更文档 | 变更类型 | 必查联动文档 | 联动原因 | 默认动作 |
|---|---|---|---|---|
| `spec.md` | 状态更新 | `sdd-status.md` | 同步当前文档状态与阶段进度 | 更新状态卡 |
| `spec.md` | 内容更新 | `plan.md`、`tasks.md`、`execution-contract.md`、`sdd-status.md` | Spec 是上游依据，变化会影响方案、任务和执行边界 | 重新检查下游文档是否仍有效，必要时回退 |
| `plan.md` | 状态更新 | `sdd-status.md` | 同步 Plan 状态 | 更新状态卡 |
| `plan.md` | 内容更新 | `tasks.md`、`execution-contract.md`、`sdd-status.md` | 方案变化会影响任务拆解和执行策略 | 重新检查任务与执行协议，必要时回退 |
| `tasks.md` | 状态更新 | `sdd-status.md` | 同步 Tasks 状态 | 更新状态卡 |
| `tasks.md` | 内容更新 | `execution-contract.md`、`review.md`、`sdd-status.md` | 任务变化会影响实现边界、review 范围与恢复动作 | 重新检查执行协议与 review 范围 |
| `execution-contract.md` | 状态更新 | `sdd-status.md` | 同步执行协议状态 | 更新状态卡 |
| `execution-contract.md` | 内容更新 | `sdd-status.md`、当前执行摘要、必要时 `tasks.md` | 执行边界、汇报规则、验证要求变化会影响当前实现 | 同步状态，检查当前实现是否仍按有效协议推进 |
| `review.md` | 状态更新 | `sdd-status.md` | 同步 review 状态 | 更新状态卡 |
| `review.md` | 内容更新 | `sdd-status.md`、`tasks.md`、下一轮入口文档 | review 结论会影响项目收口、遗留项与下一轮迭代入口 | 同步结论与未完成项 |
| `sdd-status.md` | 状态更新 | 对应主文档（必要时） | 状态卡不应长期领先或落后于主文档 | 检查主文档状态是否一致 |
| `sdd-status.md` | 内容更新 | 当前阶段主文档、必要时 checklist | 状态卡中的阶段/中断点/下一步动作必须有依据 | 校验状态卡是否与主文档自洽 |

---

## 3. 主模板与 checklist 的联动关系

| checklist | 对应主文档 | checklist 变化后应检查 | 原因 | 默认动作 |
|---|---|---|---|---|
| `specify-checklist.md` | `spec.md` | `spec.md`、`sdd-status.md` | 检查结果会影响是否进入 Plan | 若通过，更新 Spec 状态与状态卡 |
| `plan-checklist.md` | `plan.md` | `plan.md`、`sdd-status.md` | 检查结果会影响是否进入 Tasks | 若通过，更新 Plan 状态与状态卡 |
| `tasks-checklist.md` | `tasks.md` | `tasks.md`、`execution-contract.md`、`sdd-status.md` | 检查结果会影响是否可进入执行协议与 Implement 准备 | 若通过，推进执行协议 |
| `implement-handoff-checklist.md` | `execution-contract.md` | `execution-contract.md`、`sdd-status.md` | 检查结果会影响是否正式进入 Implement | 若通过，切换状态与下一步动作 |
| `review-checklist.md` | `review.md` | `review.md`、`sdd-status.md`、必要时 `tasks.md` | 检查结果会影响验收结论是否收口 | 若通过，形成正式 review 结论 |

---

## 4. 阶段切换时的最低联动要求

### 4.0 通用阶段切换动作清单（适用于多个阶段切换）
当任一阶段门禁通过、准备进入下游阶段时，默认至少执行以下动作：
1. 确认当前阶段主文档已正式落盘到项目中
2. 将当前阶段主文档头部状态回写为“已确认”
3. 若文件名承载草案/初稿等状态语义，检查并同步完成改名；若文件名为稳定命名，则明确无需改名
4. 同步更新 `sdd-status.md` 中的：当前阶段、文档状态、当前中断点、下一步唯一推荐动作、恢复提示
5. 再开始起草或推进下游阶段文档

默认原则：
> 先完成当前阶段收口，再进入下游阶段；不得跳过状态、命名、状态卡同步而直接开始下游阶段。

### 4.1 需求讨论 → Specify
至少同步：
- `spec.md`
- `sdd-status.md`

### 4.2 Specify → Plan
前置门槛：
- `spec.md` 已正式落盘到项目中；若仍只存在于对话草案中，不得进入本步骤

至少同步：
- `spec.md` 头部状态回写为“已确认”
- 若文件名承载草案语义，检查并同步完成改名
- `plan.md`（起草或准备起草）
- `sdd-status.md`

### 4.3 Plan → Tasks
前置门槛：
- `plan.md` 已正式落盘到项目中；若仍只存在于对话草案中，不得进入本步骤

至少同步：
- `plan.md` 头部状态回写为“已确认”
- 若文件名承载草案语义，检查并同步完成改名
- `tasks.md`（起草或准备起草）
- `sdd-status.md`

### 4.4 Tasks → Execution Contract
前置门槛：
- `tasks.md` 已正式落盘到项目中；若仍只存在于对话草案中，不得进入本步骤

至少同步：
- `tasks.md` 头部状态回写为“已确认”
- 若文件名承载草案语义，检查并同步完成改名
- `execution-contract.md`
- `sdd-status.md`

### 4.5 Execution Contract → Implement
前置门槛：
- `execution-contract.md` 已正式落盘到项目中；若仍只存在于对话草案中，不得进入本步骤

至少同步：
- `execution-contract.md` 头部状态回写为“已确认”
- 若文件名承载草案语义，检查并同步完成改名
- `sdd-status.md`
- 当前唯一推荐动作
- 当前执行范围说明

### 4.6 Implement → Review
至少同步：
- `review.md`（起草或准备起草）
- `sdd-status.md`
- 任务完成情况
- 验证结果摘要

---

## 5. 实现推进期的联动规则

实现推进是最容易出现“代码改了、文档没跟”的阶段，因此需要单独强调。

### 5.1 每完成一个大 task group，至少同步
- `sdd-status.md`
- 当前任务状态
- 当前验证情况
- 下一步动作
- 当前阻塞（如有）

### 5.2 若实现中发现前提不成立，至少同步
- `sdd-status.md`
- `execution-contract.md`（如执行规则已不足）
- `tasks.md`（如任务定义已不足）
- 必要时 `plan.md` 或 `spec.md`

### 5.3 若只是局部代码调试、未形成阶段性进展
默认可不更新主文档，但不得长期积累成“代码已明显推进、文档仍无反映”的状态。

---

## 6. 新对话恢复时的联动检查顺序

新对话恢复时，默认按以下顺序检查联动是否自洽：

1. 先看 `sdd-status.md`
2. 再看当前阶段主文档
3. 再看对应 checklist（若存在）
4. 再看上下游是否存在明显状态冲突
5. 若冲突存在，优先修正文档而不是依赖记忆继续推进

---

## 7. 内容更新的回退规则

以下情况默认不应只做局部修补，而应考虑阶段回退：

### 7.1 `spec.md` 内容更新
若影响：
- 目标
- 范围边界
- 用户故事主优先级
- FR / SC

则默认应重新检查：
- `plan.md`
- `tasks.md`
- `execution-contract.md`

必要时回退到 Plan 或 Tasks 重做。

---

### 7.2 `plan.md` 内容更新
若影响：
- 模块划分
- 技术路径
- 结构决策
- MVP 路径

则默认应重新检查：
- `tasks.md`
- `execution-contract.md`

必要时回退到 Tasks 甚至重新审核 Plan。

---

### 7.3 `tasks.md` 内容更新
若影响：
- 优先级
- 依赖关系
- 完成定义
- 独立验证方式

则默认应重新检查：
- `execution-contract.md`
- `review.md` 范围
- `sdd-status.md` 当前中断点与下一步动作

---

## 8. 文件名状态与文档头状态的联动规则

默认建议核心文档使用固定文件名，不依赖文件名表达状态。  
若某项目确实采用文件名状态标识，例如：

- `spec.draft.md`
- `spec.committed.md`

则发生状态变更时，必须同步更新：

1. 文件名
2. 文档头状态
3. `sdd-status.md`

三者不得长期不一致。

---

## 9. 最低执行纪律总结

若不确定某次更新是否需要大范围联动，默认至少执行以下动作：

1. 更新当前主文档  
2. 更新 `sdd-status.md`  
3. 检查一个上游文档是否仍有效  
4. 检查一个下游文档是否需要同步  
5. 若发现文档集不自洽，先修正文档，再继续推进

---

## 10. 当前版本结论

本矩阵作为当前 SDD 模板体系的联动更新初稿。  
后续若与具体模板、skill、自动起草流程结合，应保持以下原则不变：

- 状态更新必须及时
- 内容更新必须检查上下游影响
- `sdd-status.md` 必须作为主锚点维护
- 代码推进与文档推进必须尽量同步闭环
- 新对话恢复优先依赖文档集自洽，而不是依赖记忆补足
