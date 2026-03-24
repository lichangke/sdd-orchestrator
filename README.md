# sdd-orchestrator

一个面向真实项目推进的 **Spec-Driven Development（SDD）编排型 skill**。

它不是只提供几份 `spec / plan / tasks` 模板，
而是把以下能力一起收进一套可复用的协作框架：

- 阶段识别与阶段门禁
- 阶段主文档起草与收口
- `sdd-status.md` 驱动的中断恢复
- 文档联动更新纪律
- Implement 前 handoff 准备
- Review / 验收收口
- 多轮 SDD 下的“当前轮次文档映射”

当前这一版已经基于真实项目跑过两轮 SDD 实操，并做过针对性优化。

---

## 1. 这个 skill 解决什么问题

很多 SDD 实践只有模板，没有编排；
真实项目里最容易出问题的，往往不是“不会写 spec”，而是：

- 当前到底处于哪个阶段
- 当前阶段文档是否真的已经成立
- 能不能进入下一阶段
- 阶段确认后哪些状态需要同步回写
- 中断后应该从哪里恢复
- `review.md`、`sdd-status.md` 这种固定入口文档，和 `spec-002 / plan-002` 这种编号文档怎么对应
- 文档语义已经收口了，但仓库是否真的收成了稳定快照

`sdd-orchestrator` 的目标，就是把这些高频失误点显式纳入一套编排规则，而不是继续靠记忆和临场判断硬扛。

---

## 2. 当前能力边界

这个 skill 负责：

- 新项目进入 SDD 的起步编排
- 功能级 SDD 的增量推进
- Specify / Plan / Tasks / Execution Contract / Review 文档编排
- 阶段切换门禁检查
- `sdd-status.md` 状态卡维护
- 中断恢复与文档自洽校验
- Implement 前 handoff 准备
- Review / 验收的结构化收口

这个 skill **不负责**：

- 具体技术实现细节本身
- 某种语言或框架的编码规范
- 大量代码级 review 细则
- 直接替代 Coding Agent 做实现施工

一句话说：

> 它负责把 SDD 协作链路编排清楚，但不负责替代实现层。

---

## 3. 当前版本的设计重点

相较于普通模板包，这一版重点强化了以下能力：

### 3.1 阶段主文档必须先落盘
对话中的草案、骨架、口头收口，不等于项目内已存在的阶段主文档。
如果当前阶段主文档尚未正式落盘，就不能口头进入下一阶段。

### 3.2 阶段确认后的状态回写不可遗漏
当阶段门禁通过时，必须先同步：

- 当前阶段主文档头部状态
- 若文件名表达草案语义，则检查是否需改名
- `sdd-status.md`

再开始下游阶段。

### 3.3 文档头元信息必须和正文用途一致
如果 `review.md` 或其他固定入口文档已经切换到当前轮次，
则文档名称、所属功能 / 子功能、上游文档、时间等头部字段也必须一起切换，
不能只换正文。

### 3.4 支持多轮 SDD 的文档映射
当前默认允许：

- `spec / plan / tasks / execution-contract` 使用编号文档
- `review.md` / `sdd-status.md` 作为固定入口文档

并要求在状态卡中明确“当前有效轮次文档”与固定入口文档的映射关系。

### 3.5 Review 收口支持真实复杂分支
这一版已经明确支持：

- 有条件通过 / 阶段性通过
- 条件项是否阻断当前主线验收
- 外部失败（如 provider cooldown）如实记录
- 再次重试仍失败后的真实收口
- 当前轮次已收口但暂不开启下一轮

### 3.6 收口不等于自动完成仓库快照
当已经形成“已收口 / 已通过 / 已完成”的语义时，
还需要继续检查当前工作区是否存在与本轮收口直接相关的未提交改动。

---

## 4. 目录结构

```text
sdd-orchestrator/
├── README.md
├── SKILL.md
└── references/
    ├── 索引与导航.md
    ├── SDD 模板总则.md
    ├── SDD 联动更新矩阵.md
    ├── Coding Agent Handoff 协议.md
    ├── 实现层入口与链路.md
    ├── core-templates/
    │   ├── spec.md
    │   ├── plan.md
    │   ├── tasks.md
    │   ├── execution-contract.md
    │   ├── sdd-status.md
    │   └── review.md
    └── checklists/
        ├── specify-checklist.md
        ├── plan-checklist.md
        ├── tasks-checklist.md
        ├── implement-handoff-checklist.md
        ├── review-checklist.md
        └── document-header-checklist.md
```

---

## 5. 建议阅读顺序

### 第一步：看 `SKILL.md`
它负责：
- 触发场景
- 核心硬规则
- 场景分流
- 最小读取路径

### 第二步：看 `references/索引与导航.md`
它负责：
- references 内部导航
- 文件角色说明
- 不同场景该先读哪些文件

### 第三步：按场景进入对应文件

#### 如果你要理解上位规则
优先看：
- `references/SDD 模板总则.md`
- `references/SDD 联动更新矩阵.md`

#### 如果你要推进阶段文档
优先看：
- 当前阶段主模板
- 当前阶段 checklist
- `references/core-templates/sdd-status.md`

#### 如果你要做 Review / 验收收口
优先看：
- `references/core-templates/review.md`
- `references/checklists/review-checklist.md`
- 必要时 `references/checklists/document-header-checklist.md`

#### 如果你要处理中断恢复或文档自洽问题
优先看：
- `references/core-templates/sdd-status.md`
- `references/SDD 联动更新矩阵.md`
- 必要时 `references/checklists/document-header-checklist.md`

---

## 6. 安装方式（面向 OpenClaw 工作区）

如果你只是要在 OpenClaw 工作区里手动安装这个 skill，最简单的方式就是把整个目录放到：

`/root/.openclaw/workspace/skills/sdd-orchestrator`

也就是保证最终至少有：

- `/root/.openclaw/workspace/skills/sdd-orchestrator/SKILL.md`
- `/root/.openclaw/workspace/skills/sdd-orchestrator/references/...`

### 从本仓库同步到 runtime skill 的常见方式
在工作区根目录执行：

```bash
mkdir -p /root/.openclaw/workspace/skills/sdd-orchestrator
cp -r /root/.openclaw/workspace/sdd-orchestrator/* /root/.openclaw/workspace/skills/sdd-orchestrator/
```

如果你已经在维护仓库版与 runtime skill 双份目录，建议每次更新后至少校验这些关键文件是否一致：

- `SKILL.md`
- `references/SDD 模板总则.md`
- `references/SDD 联动更新矩阵.md`
- `references/core-templates/sdd-status.md`
- `references/core-templates/review.md`
- `references/checklists/review-checklist.md`
- `references/checklists/document-header-checklist.md`
- `references/索引与导航.md`

---

## 7. 维护约定

如果你同时维护：

- 仓库版：`/root/.openclaw/workspace/sdd-orchestrator`
- runtime skill：`/root/.openclaw/workspace/skills/sdd-orchestrator`

请注意：

> 修改仓库版，不等于 runtime 已生效。

默认维护约定应为：

1. 先改仓库版
2. 再同步到 runtime skill
3. 再校验关键文件一致
4. 仅仓库版做 commit / push
5. runtime skill 目录只做同步，不单独作为仓库发布对象

---

## 8. 推荐使用方式

当前更推荐的使用姿势不是“继续空转优化模板”，而是：

1. 用这版 skill 去跑真实项目
2. 在真实 SDD 协作里暴露问题
3. 只修改真实出现的高频问题
4. 避免重新退回“凭想象补规则”的状态

也就是说，这个 skill 的后续迭代应当：

> 以真实实操反馈驱动，而不是以抽象打磨驱动。

---

## 9. 当前状态

当前这一版可以视为：

> **已跑过两轮真实 SDD 的可用基线版本。**

它不是终态，但已经不再只是“模板集合”，而是一套可以在真实项目中使用的 SDD 编排框架。

---

## 10. 一句话总结

> `sdd-orchestrator` 不是几份文档模板，而是一套把阶段推进、状态维护、恢复、自检、联动和 Review 收口一起管起来的 SDD 编排 skill。
