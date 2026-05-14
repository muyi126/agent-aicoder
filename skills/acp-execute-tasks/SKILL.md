---
name: acp-execute-tasks
description: 在用户说"执行任务"、"按任务清单实现"、"基于方案写代码"、或"开始编码"时使用。同时适用于：execute tasks、implement based on plan、start coding from technical specification。
version: 1.0.0
rules:
  - aicoder/rules/generate-code.mdc
  - aicoder/rules/microservice-dev-standards.mdc
  - aicoder/rules/code-review.mdc
input:
  - aicoder/.current_work/{service_name}/solution.md（方案文档）
  - aicoder/.current_work/{service_name}/task-list.md（任务清单）
  - aicoder/.current_work/{service_name}/need.md（需求文档）
  - aicoder/.current_work/{service_name}/analyze.md（分析文档）
output:
  - 与任务绑定的代码变更
service_name_required: true
---

# Execute Tasks Skill

基于已批准的方案和任务清单执行代码实现。目标是精确按照计划实现代码，不添加创意或偏离。

---

## 工作流程

### 1. 评审计划

在写任何代码前，评审：
- 方案文档（solution.md）
- 任务清单（task-list.md）
- 需求文档（need.md）
- 分析文档（analyze.md）
- 现有代码风格和模式

### 2. 匹配需求

验证：
- 任务清单覆盖所有需求
- 方案与契约一致
- 文档间无矛盾

若发现矛盾，立即标记并请求澄清。

### 3. 按顺序执行任务

每个任务：
- 读取描述
- 参考方案和现有风格
- 生成/修改最小必要代码
- 验证编译/语法
- 标记任务完成

### 4. 验证每步

每任务后：
- 运行相关检查（测试、类型检查、lint、构建）
- 验证未修改无关文件
- 文档化发现的问题

### 5. 处理偏离

若计划在执行中揭示问题：
- 详细文档化问题
- 提出修复建议
- 未经批准不得偏离计划

---

## 输出格式

```text
已执行任务:
- [x] 任务 1 - 验证方式 <检查>
- [x] 任务 2 - 验证方式 <检查>
...

阻塞任务:
- [ ] 任务 N - 原因

发现问题:
- ...
```

---

## Guardrails

- 只执行批准列表中的任务
- 不添加计划中未包含的功能
- 不重构无关代码
- 若发现有问题，停止并解释后再继续