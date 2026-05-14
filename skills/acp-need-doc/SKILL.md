---
name: acp-need-doc
description: 在用户说"写需求"、"生成需求文档"、"need"、"需求文档"、或描述一个功能/业务希望产出需求文档时使用。需指定服务名（如 frontend-task-display）。同时适用于：生成 requirement document、create a need document、write requirements。
version: 1.0.0
rules:
  - aicoder/rules/microservice-standards-core.mdc
  - aicoder/rules/architecture-guide.mdc
input:
  - 用户的文字描述（业务场景、目标、功能点、约束等）
  - aicoder/.current_work/{service_name}/need.md（需参考模板结构时）
output:
  - aicoder/.current_work/{service_name}/need.md
service_name_required: true
---

# Need Doc Skill

使用本 skill 生成正式需求文档，捕获业务问题、目标和验收标准。

---

## 工作流程

### 1. 解析用户描述

提取：
- 业务场景或要解决的问题
- 核心概念和术语
- 目标和成功标准
- 约束和边界
- 受影响的用户或系统

### 2. 加载规则约束

读取 `aicoder/rules/` 下相关规则（如 `microservice-standards-core.mdc`、`architecture-guide.mdc`、`project-analyze.mdc`），确保术语与边界一致。

### 3. 文档结构

必须包含以下章节，顺序与层级保持一致：

```markdown
# [需求主题] 需求文档

## 需求背景
（问题/现状简述；可含业务场景示例）

## 业务目标
（可多条，每条清晰可衡量）

## 需求描述
（按功能/模块分节，如 6. xxx 功能；含核心概念、类型定义、流转规则等子节）

## 验收标准（[主题]）
（基础功能、类型功能、关键流程等分块，用 - [ ] 清单）

## 相关文件（[主题]）
（数据库 / 后端 / 前端 / 文档 分类列出路径与说明）

## 实施优先级（[主题]）
（P0/P1/P2 等分级与说明）
```

### 4. 对齐现有系统

若涉及现有模块、表名、API，查阅 `aicoder/docs/` 中对应文档，保证命名与分层一致。

### 5. 输出

保存到 **`aicoder/.current_work/{service_name}/need.md`**（覆盖写入）。

---

## Guardrails

- 若用户描述过于简略，在对应节标注「待细化」并保留结构
- 术语必须与 `aicoder/rules` 及现有文档一致，避免自造术语
- 路径使用正斜杠，如 `aicoder/.current_work/frontend-task-display/need.md`

## 与其他 Skill 的关系

- 本 skill 产出 need.md 后，可作为后续阶段的输入：
  - **契约**: `acp-contract-design` 的输入
  - **方案**: `acp-solution-design` 的输入
  - **研究**: `init-workflow`（本 skill 先于 init-workflow 执行）