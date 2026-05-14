---
name: acp-contract-design
description: 在用户说"写契约"、"接口契约"、"契约设计"、或"基于分析和需求定义服务边界与接口"时使用。同时适用于：define service boundaries、design interfaces、write contracts。
version: 1.0.0
rules:
  - aicoder/rules/microservice-standards-core.mdc
  - aicoder/rules/microservice-understanding-rule.mdc
input:
  - aicoder/.current_work/{service_name}/need.md（需求文档）
  - aicoder/.current_work/{service_name}/analyze.md（分析文档）
output:
  - aicoder/.current_work/{service_name}/contract.md
service_name_required: true
---

# Contract Design Skill

在实现之前建立服务之间的清晰契约。定义业务流、域所有权、状态机和接口定义。

---

## 工作流程

### 1. 评审输入

在设计契约前，评审：
- 需求文档（need.md）
- 分析文档（analyze.md）
- 现有服务接口和数据模型
- 域所有权和责任边界

### 2. 定义业务流

文档化关键业务流：
- 入口点和出口点
- 决策点和状态转换
- 错误处理路径
- 异步操作和回调

### 3. 定义域所有权

澄清：
- 哪个服务拥有哪个数据
- 哪个服务负责哪些业务规则
- 如何在服务间保持数据一致性

### 4. 定义接口契约

每个接口需定义：
- 请求/响应结构（DTO/VO）
- 认证和授权要求
- 错误码和错误处理约定
- 幂等性和重试策略
- 超时和限流

### 5. 定义状态机

如果业务涉及状态转换：
- 定义所有可能状态
- 定义有效转换
- 定义转换触发器
- 定义转换的副作用

### 6. 创建术语表

定义统一术语表以避免团队间的歧义。

---

## 输出格式

```markdown
# 契约文档 - [服务/功能名称]

## 1. 业务流
## 2. 域所有权
## 3. 接口契约
## 4. 状态机
## 5. 术语表
## 6. 约束和假设
```

---

## Guardrails

- 契约应该是技术无关的（关注是什么，而非怎么做）
- 明确文档化假设
- 定义错误码时与现有项目保持一致
- 实施前获得相关方确认