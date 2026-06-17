---
name: "flow-automation"
description: "Automates multi-step workflows with task planning, execution, monitoring, and reporting. Invoke when user explicitly requests automation, workflow execution, or batch processing."
---

# Flow Automation

> **📋 必读文件清单（执行前必须全部读取）**
>
> | 序号 | 文件 | 路径 | 用途 |
> |------|------|------|------|
> | 1 | 工作流模式 | `./templates/工作流模式.md` | 7种自动化工作流模式定义 |
> | 2 | 异常处理表 | `./templates/异常处理表.md` | 错误处理参考 |
> | 3 | 修复优先级表 | `./templates/修复优先级表.md` | 修复优先级参考 |
>
> **标准来源**：本技能引用 `novel-writer-master/统一标准.md` v2.9（2026-05-29）
>
> **❌ 禁止跳过模板读取**：模板文件包含工作流模式和异常处理规则，不读取将导致执行不完整。

This skill provides comprehensive automation workflow capabilities for planning, executing, monitoring, and reporting multi-step tasks.

## Core Capabilities

### 1. Task Auto-Planning
- Automatically decompose complex tasks into executable steps
- Identify dependencies between steps
- Optimize execution order for efficiency
- Generate structured task plans

### 2. Workflow Auto-Execution
- Execute planned steps sequentially or in parallel
- Handle inter-step data flow
- Manage execution context and state
- Support rollback on failure

### 3. Status Monitoring & Adjustment
- Real-time progress tracking
- Automatic error detection and handling
- Dynamic workflow adjustment based on conditions
- Resource usage monitoring

### 4. Result Report Generation
- Comprehensive execution summaries
- Success/failure analysis
- Performance metrics
- Recommendations for improvement

## When to Invoke

This skill should be triggered when:
- User explicitly requests automation: "自动化执行这个任务"
- User asks for workflow creation: "创建一个工作流"
- User needs batch processing: "批量处理这些文件"
- User wants task planning: "帮我规划这个任务的执行步骤"

---

## Workflow Execution Modes

> 完整的7种工作流模式定义详见 [工作流模式](./templates/工作流模式.md)。

### Mode 1: Sequential Execution
```
Step 1 → Step 2 → Step 3 → ... → Completion
```
Use when: Steps have clear dependencies

### Mode 2: Parallel Execution
```
Step 1 ─┬→ Step 2a ─┐
        ├→ Step 2b ─┼→ Step 3 → Completion
        └→ Step 2c ─┘
```
Use when: Steps are independent and can run simultaneously

### Mode 3: Conditional Execution
```
Step 1 → [Condition Check] → (True) → Step 2a → Completion
                        └→ (False) → Step 2b → Completion
```
Use when: Execution path depends on intermediate results

---

## Task Plan Structure

```json
{
  "task_id": "unique-task-identifier",
  "task_name": "Human-readable task name",
  "steps": [
    {
      "step_id": 1,
      "step_name": "Step description",
      "dependencies": [],
      "execution_mode": "sequential|parallel|conditional",
      "estimated_duration": "time estimate",
      "required_resources": ["resource1", "resource2"]
    }
  ],
  "overall_estimated_duration": "total time estimate",
  "risk_factors": ["risk1", "risk2"],
  "fallback_plan": "Alternative approach if main plan fails"
}
```

---

## Execution Report Format

```markdown
# Task Execution Report

## Summary
- **Task ID**: {task_id}
- **Status**: {success|partial_success|failure}
- **Duration**: {actual_time}
- **Completion Rate**: {percentage}

## Step-by-Step Results
| Step | Status | Duration | Notes |
|------|--------|----------|-------|
| 1 | ✓ | 5min | Completed successfully |
| 2 | ✓ | 10min | Completed successfully |
| 3 | ✗ | 2min | Failed due to... |

## Issues Encountered
1. Issue description and resolution

## Recommendations
1. Suggested improvements for future executions
```

---

## Best Practices

> 错误处理策略详见 [异常处理表](./templates/异常处理表.md)，修复优先级详见 [修复优先级表](./templates/修复优先级表.md)。

1. **Always validate inputs** before starting workflow execution
2. **Set reasonable timeouts** for each step to prevent hanging
3. **Implement proper error handling** with clear fallback strategies
4. **Monitor resource usage** to prevent system overload
5. **Generate detailed logs** for debugging and auditing
6. **Follow unified standards** defined in `novel-writer-master/统一标准.md`

---

## Integration with Novel Writing Workflow

This skill integrates with the novel writing workflow:

1. **Outline Design Phase**: Automate batch outline creation
2. **Chapter Outline Phase**: Automate chapter outline generation
3. **Writing Phase**: Automate daily writing tasks
4. **Review Phase**: Automate quality checks

All automated workflows must respect:
- Daily target: 4200 non-blank characters
- Chapter target: ≥2100 non-blank characters
- Beat intervals: Small (3-5 chapters) / Medium (20-30 chapters) / Large (50-100 chapters)
