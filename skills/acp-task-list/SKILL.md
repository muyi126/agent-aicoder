---
name: acp-task-list
description: 在用户说"生成任务清单"、"分解方案为可执行任务"、或"创建 todo 列表"时使用。同时适用于：generate a task list、decompose into tasks、create a todo list。
version: 1.0.0
rules:
  - aicoder/rules/microservice-dev-standards.mdc
input:
  - aicoder/.current_work/{service_name}/solution.md（方案文档）
  - aicoder/.current_work/{service_name}/contract.md（契约文档）
output:
  - aicoder/.current_work/{service_name}/task-list.md
service_name_required: true
---

# Task List Skill

将方案或技术规范分解为可执行的任务列表。目标是分解实现为小的、可独立验证的步骤。

---

## 工作流程

### 1. 评审方案文档

理解：
- 整体架构
- 实施任务
- 组件间的依赖
- 验收标准

### 2. 分解为原子任务

每个任务应该：
- 做一个单一的概念性变更
- 可独立验证
- 有清晰的完成标准
- 可逆（如果可能）

### 3. 排序任务

考虑：
- 依赖（什么必须先做）
- 风险（从高置信度项开始）
- 验证顺序（可能的话尽早测试）

### 4. 为每个任务定义验证

指定：
- 如何验证完成
- 运行什么测试
- 代码审查中检查什么

---

## 输出格式

```markdown
## 实施任务清单

- [ ] **任务 1**: <描述> — 验证方式 <如何>
- [ ] **任务 2**: <描述> — 验证方式 <如何>
...

## 任务依赖
- 任务 N 需要任务 M
```

---

## Guardrails

- 任务应该小到可以在一会话中完成
- 避免跨越多个文件的任务（除非必要）
- 每个任务应有清晰的完成定义
- 参考代码库中现有模式