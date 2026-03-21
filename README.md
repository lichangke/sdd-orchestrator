# sdd-orchestrator

一个面向长期人机协作的 **SDD（Spec-Driven Development）编排型 skill 包**。  
它不是单纯的 spec / plan / tasks 模板集合，而是试图把以下能力收成一套可复用的协作框架：

- 需求澄清与阶段推进
- Spec / Plan / Tasks / Review 文档编排
- 阶段门禁与 checklist 检查
- `sdd-status.md` 驱动的中断恢复
- 文档联动更新纪律
- Implement 前 handoff 准备
- Coding Agent / 子 agent 的实现层交接链路

当前状态：**可试运行定稿**。

---

## 1. 这个仓库解决什么问题

很多“Spec-Driven Development”实践只提供几份模板，但在真实协作里还缺很多关键东西：

- 什么时候该进入下一阶段
- 草案和定稿怎么区分
- 中断之后怎么恢复
- 文档变了以后要不要联动更新别的文档
- Implement 阶段怎么交给 Coding Agent / 子 agent
- 如何避免“文档写了很多，但协作还是断裂”

`sdd-orchestrator` 的目标，就是把这些问题一起纳入同一套编排体系。

---

## 2. 当前目录结构

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
    ├── sdd-orchestrator 首次试运行方案.md
    ├── core-templates/
    │   ├── 模板说明.md
    │   ├── spec.md
    │   ├── plan.md
    │   ├── tasks.md
    │   ├── execution-contract.md
    │   ├── sdd-status.md
    │   └── review.md
    └── checklists/
        ├── 清单说明.md
        ├── specify-checklist.md
        ├── plan-checklist.md
        ├── tasks-checklist.md
        ├── implement-handoff-checklist.md
        └── review-checklist.md
```

---

## 3. 使用方式（当前建议）

### 第一步：从 `SKILL.md` 进入
`SKILL.md` 负责：
- 触发描述
- 场景分流
- 最小规则
- 该读哪些文件

### 第二步：从 `references/索引与导航.md` 进入 references
它负责：
- 目录结构导航
- 文件类别说明
- 不同场景下的读取顺序

### 第三步：按场景进入对应文件
例如：

- 理解规则 → `SDD 模板总则.md` / `SDD 联动更新矩阵.md`
- 起草阶段文档 → `core-templates/` + `checklists/`
- 恢复项目 → `core-templates/sdd-status.md`
- 进入 Implement → `execution-contract.md` + `implement-handoff-checklist.md` + `Coding Agent Handoff 协议.md`

---

## 4. 当前设计重点

这套 skill 目前强调以下几点：

### 4.1 草案不等于定稿
阶段文档在用户明确确认前，不默认视为当前有效依据。

### 4.2 不越阶段
未完成当前阶段收口前，不进入下一阶段。

### 4.3 `sdd-status.md` 是恢复锚点
恢复优先依赖项目文档，不依赖聊天上下文记忆。

### 4.4 文档联动更新是第一类规则
文档变化后必须检查相关文档同步，而不是只改一处。

### 4.5 Implement 不是黑箱
通过 `execution-contract.md`、handoff checklist 与 handoff 协议，把实现层纳入整体协作链路。

---

## 5. 当前版本状态

当前仓库状态可定义为：

> **可试运行定稿**

意思是：
- 文档结构已经收口
- 目录与命名已基本稳定
- 已具备真实项目试运行条件
- 后续迭代应主要来自真实使用反馈，而不是继续抽象空转

---

## 6. 推荐下一步

当前最自然的下一步有两个：

### A. 做真实试运行
优先建议以 `local-kb-assistant` 作为第一试运行对象，验证：
- 编排层
- 模板层
- checklist 层
- handoff 链路
- 恢复与联动机制

### B. 按试运行反馈迭代 skill
只修改真实暴露的问题，避免继续无边界地打磨文档。

---

## 7. 相关文档入口

- skill 主入口：`SKILL.md`
- references 总入口：`references/索引与导航.md`
- 实现层入口：`references/实现层入口与链路.md`
- handoff 协议：`references/Coding Agent Handoff 协议.md`
- 首次试运行方案：`references/sdd-orchestrator 首次试运行方案.md`

---

## 8. 一句话总结

> `sdd-orchestrator` 不是“几份模板”，而是一套面向长期人机协作的 SDD 编排框架：既管文档阶段，也管恢复、联动、handoff 与实现层链路。
