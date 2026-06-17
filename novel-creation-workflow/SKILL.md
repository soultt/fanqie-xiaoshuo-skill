---
name: "novel-creation-workflow"
description: "完整的小说创作流程入口，调用 novel-writer-master 执行全流程自动化创作。Invoke when user wants to start a complete novel creation workflow, or manage the full writing process."
---

# 小说完整创作流程

> **📋 必读文件清单（执行前必须全部读取）**
>
> | 序号 | 文件 | 路径 | 用途 |
> |------|------|------|------|
> | 1 | 日更实战方案 | `./templates/日更实战方案.md` | 日更4200字符时间安排和策略 |
> | 2 | 章纲模板 | `./templates/章纲模板.md` | 章纲快速参考格式 |
> | 3 | 审查报告模板 | `./templates/审查报告模板.md` | 各类审查报告格式模板 |
> | 4 | 签约评估模板 | `../fanqie-contract-evaluator/templates/签约评估模板.md` | 签约评估报告格式 |
>
> **标准来源**：本技能引用 `novel-writer-master/统一标准.md` v2.9（2026-05-29）
>
> **❌ 禁止跳过模板读取**：模板文件包含日更策略、章纲格式和审查报告模板，不读取将导致流程不完整。

## 核心定位

本技能是小说创作全流程的**入口技能**，负责调用 `novel-writer-master` 执行完整的自动化创作流程。

> **技能边界说明（强制）**：本技能不直接执行创作任务，而是通过调用 `novel-writer-master` 技能来完成所有工作。`novel-writer-master` 是实际的执行者，包含完整的流程定义、技能调用链和质量控制机制。

## 使用方式

当用户请求开始小说创作时，本技能会：

1. **调用 novel-writer-master 技能**
   ```
   使用 skill 工具调用 novel-writer-master
   → 传入参数：创作需求（类型、主题、风格、预计字数等）
   → 等待技能执行完成
   → 接收返回结果
   ```

2. **novel-writer-master 将自动执行以下流程**：
   - 阶段一：项目初始化
   - 阶段二：设定设计（调用 novel-setting-designer）
   - 阶段三：大纲设计（调用 novel-outline-designer）
   - 阶段四：大纲审查（调用 novel-master-outline-vetter）
   - 阶段五：章纲创建（调用 novel-chapter-outline-creator）
   - 阶段六：章纲审查与修复（调用 novel-outline-vetter）
   - 阶段七：正文写作（调用 novel-chapter-writer）
   - 阶段八：正文审查与修复（调用 novel-chapter-vetter）
   - 阶段九：签约评估（调用 fanqie-contract-evaluator）

## 技能关系

```
novel-creation-workflow (入口技能)
    │
    └──→ novel-writer-master (总控技能)
              │
              ├──→ novel-setting-designer (设定设计)
              ├──→ novel-outline-designer (大纲设计)
              ├──→ novel-master-outline-vetter (大纲审查)
              ├──→ novel-chapter-outline-creator (章纲创建)
              ├──→ novel-outline-vetter (章纲审查)
              ├──→ novel-chapter-writer (正文写作)
              ├──→ novel-chapter-vetter (正文审查)
              └──→ fanqie-contract-evaluator (签约评估)
```

## 项目目录结构

完整8个目录结构（与统一标准一致）：

```
小说项目/
├── 01-总纲/                          ← 大纲设计阶段
├── 02-卷纲/                          ← 大纲设计阶段
├── 03-人物设定/                      ← 设定设计阶段
├── 04-世界观/                        ← 设定设计阶段
├── 05-章纲/                          ← 章纲规划阶段
├── 06-正文/                          ← 正文写作阶段
├── 07-审查报告/                      ← 质量审查阶段
└── 08-约束清单/                      ← 约束管理阶段
```

所有卷目录使用 `NN-第X卷-卷名/` 格式。

## 详细文档

完整的流程定义、质量控制机制、异常处理等详见：
- [novel-writer-master 技能文档](../novel-writer-master/SKILL.md)
- [统一标准规范](../novel-writer-master/统一标准.md)

## 模板与引用

> **模板引用**：以下模板文件为本技能提供参考模板，实际创作时由子技能调用。

| 模板文件 | 用途 | 引用技能 |
|----------|------|----------|
| [日更实战方案](./templates/日更实战方案.md) | 日更4200字符时间安排和策略 | 本技能参考 |
| [审查报告模板](./templates/审查报告模板.md) | 各类审查报告格式模板 | 子技能参考 |
| [章纲模板](./templates/章纲模板.md) | 章纲快速参考格式（权威模板见子技能） | 快速参考 |
| [签约评估模板](../fanqie-contract-evaluator/templates/签约评估模板.md) | 签约评估报告格式 | fanqie-contract-evaluator |

## 使用注意事项

1. 本技能是入口技能，实际执行由 `novel-writer-master` 完成
2. 所有数值标准以 `统一标准.md` 为准
3. 如需单独执行某个阶段，可直接调用对应的子技能
