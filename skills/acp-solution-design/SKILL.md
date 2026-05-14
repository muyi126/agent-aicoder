---
name: acp-solution-design
description: 在用户说"方案"、"技术方案"、"写方案"、或"基于需求和契约生成技术规范"时使用。同时适用于：create a technical solution、design a solution、write a solution document。
version: 1.0.0
rules:
  - aicoder/rules/architecture-guide.mdc
  - aicoder/rules/channel-dev-rule.mdc（如适用）
input:
  - aicoder/.current_work/{service_name}/need.md（需求文档）
  - aicoder/.current_work/{service_name}/analyze.md（分析文档）
  - aicoder/.current_work/{service_name}/contract.md（契约文档）
output:
  - aicoder/.current_work/{service_name}/solution.md
service_name_required: true
---

# Solution Design Skill

基于需求和契约创建详细技术方案文档。产出涵盖架构、接口、数据模型和验收标准的综合实施计划。

---

## 工作流程

### 1. 评审输入

- 需求文档（need.md）
- 契约文档（contract.md）
- 分析文档（analyze.md）
- 现有代码模式和架构

### 2. 定义整体架构

- 高层组件图
- 组件间数据流
- 技术选型及理由
- 非功能性需求（性能、可扩展性、可靠性）

### 3. 设计接口规格

每个接口需定义：
- 端点定义（URL、方法）
- 请求/响应模式
- 认证/授权
- 错误处理
- 示例

### 4. 设计数据模型

- 数据库 schema 变更
- 实体关系
- 迁移策略
- 数据所有权和一致性

### 5. 定义实施步骤

分解为原子级、可验证的任务：
- 每个任务应可独立测试
- 每个任务应有清晰的验收标准
- 排序任务以最小化风险和依赖

### 6. 文档化风险和缓解

- 识别技术风险
- 提出缓解措施
- 定义回滚计划

---

## 输出格式

```markdown
# 方案文档 - [功能/服务名称]

## 1. 概述
## 2. 架构
## 3. 接口规格
## 4. 数据模型
## 5. 实施任务
## 6. 风险和缓解
## 7. 验收标准
```

---

## Guardrails

- 遵循现有项目模式和约定
- 偏好简单方案而非复杂方案
- 文档化每个假设
- 保持方案聚焦于声明的需求
- 参考 `rules/architecture-guide.mdc` 的最佳实践