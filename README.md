# sdd-orchestrator

`sdd-orchestrator` 是一个面向真实项目推进的 **Spec-Driven Development（SDD）编排型 skill**。

它的目标不是提供几份孤立的 `spec / plan / tasks` 模板，而是把项目推进里最容易失控的部分一起纳入显式规则：

- 当前处于哪个阶段
- 当前阶段能不能进入下一阶段
- 阶段文档是否真的成立
- `sdd-status.md` 如何作为恢复主锚点
- 文档变化后哪些地方需要联动检查
- Implement 前如何 handoff 给 Coding Agent
- Review / 验收如何形成结构化收口

一句话说：

> 这是一个管 **阶段推进、状态维护、恢复、自检、联动与交接** 的 SDD 编排器，而不是单纯的模板包。

---

## 1. 这个仓库里有什么

当前仓库的 skill 主体由两部分组成：

- `SKILL.md`：skill 入口，负责触发描述、核心硬规则、场景分流与最小读取路径
- `references/`：详细规则、模板、checklist 与实现层配套说明

这套结构已经可以支撑：

- 新项目进入 SDD
- 起草 / 收口 `Specify / Plan / Tasks / Execution Contract / Review`
- 阶段门禁判断
- `sdd-status.md` 驱动的恢复推进
- Implement 前 handoff 准备
- Review / 验收收口
- 多轮 SDD 下的轮次文档映射

---

## 2. skill 主体结构

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
        ├── document-header-checklist.md
        ├── 模板说明.md
        └── 清单说明.md
```

说明：

- `SKILL.md` 偏“入口层”
- `references/` 偏“规则层 / 模板层 / 门禁层 / handoff 配套层”
- 当前仓库没有额外脚本目录；核心价值主要在文档编排规则本身

---

## 3. 这个 skill 解决什么问题

很多 SDD 实践的问题不在于“不会写 spec”，而在于：

- 对话里的草稿和项目里的正式文档混用
- 阶段切换靠感觉推进，没有门禁
- `review.md`、`sdd-status.md` 与当前轮次文档脱锚
- 文档变了，下游没有同步检查
- Implement 交给 Coding Agent 后边界漂移
- Review 语义已经收口，但仓库快照并没有真正收稳

`sdd-orchestrator` 的作用，就是把这些高频失误点收成一套显式编排规则。

---

## 4. 当前设计重点

这一版重点强化了以下能力：

### 4.1 阶段主文档必须先落盘
对话中的骨架、草稿、口头收口，不能替代项目内的正式阶段主文档。

### 4.2 阶段确认后的状态回写不可遗漏
阶段门禁通过后，必须同步回写：

- 当前阶段主文档头部状态
- 必要时的文件名语义
- `sdd-status.md`

### 4.3 文档头元信息必须与正文用途一致
当固定入口文档切换到当前轮次用途时，文档头字段也必须一起切换。

### 4.4 支持多轮 SDD 的轮次映射
允许：

- `spec / plan / tasks / execution-contract` 使用编号文档
- `review.md` / `sdd-status.md` 继续作为固定入口文档

并要求在状态卡中明确当前有效轮次映射。

### 4.5 Review 收口不等于仓库快照自动稳定
形成“已收口 / 已通过 / 已完成”结论后，仍需检查当前工作区是否存在与本轮收口直接相关的未提交改动。

---

## 5. 建议阅读顺序

### 第一层：入口
先看：

- `SKILL.md`

用途：

- 判断这个 skill 在什么场景触发
- 理解核心硬规则
- 理解场景分流与最小读取路径

### 第二层：导航
再看：

- `references/索引与导航.md`

用途：

- 理解 references 内部结构
- 根据具体场景定位应该先读哪些文件

### 第三层：按任务进入对应文件

#### 如果要理解上位规则
优先看：

- `references/SDD 模板总则.md`
- `references/SDD 联动更新矩阵.md`

#### 如果要推进阶段文档
优先看：

- 对应阶段主模板
- 对应 checklist
- `references/core-templates/sdd-status.md`

#### 如果要处理中断恢复 / 状态冲突
优先看：

- `references/core-templates/sdd-status.md`
- `references/SDD 联动更新矩阵.md`
- `references/checklists/document-header-checklist.md`

#### 如果要进入 Implement 前 handoff
优先看：

- `references/core-templates/execution-contract.md`
- `references/checklists/implement-handoff-checklist.md`
- `references/Coding Agent Handoff 协议.md`
- `references/实现层入口与链路.md`

#### 如果要做 Review / 验收收口
优先看：

- `references/core-templates/review.md`
- `references/checklists/review-checklist.md`
- 必要时 `references/checklists/document-header-checklist.md`

---

## 6. 能力边界

这个 skill 负责：

- SDD 阶段编排
- 阶段门禁判断
- 文档联动检查
- `sdd-status.md` 驱动的恢复
- Implement 前 handoff 准备
- Review / 验收的结构化收口

这个 skill 不负责：

- 直接替代 Coding Agent 写具体实现
- 某种语言 / 框架本身的编码规范
- 大量代码级 review 细则
- 通用项目管理软件层面的排期与资源管理

边界总结：

> 它解决的是 **SDD 协作编排**，不是实现施工本身。

---

## 7. 如何同步到 runtime skill

如果你在同时维护：

- 仓库版：`/root/.openclaw/workspace/sdd-orchestrator`
- runtime 版：`/root/.openclaw/workspace/skills/sdd-orchestrator`

请注意：

> 修改仓库版，不等于 runtime 已生效。

推荐同步方式：

```bash
mkdir -p /root/.openclaw/workspace/skills/sdd-orchestrator
cp /root/.openclaw/workspace/sdd-orchestrator/SKILL.md /root/.openclaw/workspace/skills/sdd-orchestrator/
rm -rf /root/.openclaw/workspace/skills/sdd-orchestrator/references
cp -r /root/.openclaw/workspace/sdd-orchestrator/references /root/.openclaw/workspace/skills/sdd-orchestrator/
```

建议每次同步后至少校验这些关键文件：

- `SKILL.md`
- `references/SDD 模板总则.md`
- `references/SDD 联动更新矩阵.md`
- `references/core-templates/sdd-status.md`
- `references/core-templates/review.md`
- `references/checklists/review-checklist.md`
- `references/checklists/document-header-checklist.md`
- `references/索引与导航.md`

---

## 8. 当前使用建议

当前更推荐的使用方式不是继续抽象打磨，而是：

1. 用它跑真实项目
2. 只围绕真实暴露的问题补规则
3. 避免重新退回“凭想象补模板”的状态

也就是说，后续迭代应优先遵循：

> **以真实实操反馈驱动，而不是以抽象打磨驱动。**

---

## 9. 当前状态

当前这一版可以视为：

> **已能支撑真实项目推进的 SDD 编排基线版本。**

它不是终态，但已经不是“几份模板的集合”，而是一套具备阶段推进、状态维护、恢复、自检、联动与 handoff 规则的协作框架。

---

## 10. 一句话总结

> `sdd-orchestrator` 是一个把 **阶段推进、状态卡、恢复、自检、文档联动和 Implement 交接** 一起管起来的 SDD 编排 skill。