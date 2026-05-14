---
name: acp-update-doc
description: 在用户说"更新文档"、"同步文档"、"更新 UML"、或"代码变更后同步架构文档"时使用。同时适用于：update documentation、sync docs、update UML diagrams。
version: 1.0.0
rules:
  - aicoder/rules/architecture-guide.mdc
input:
  - git diff
  - 代码评审结论
output:
  - 更新的文档
service_name_required: false
---

# Update Doc Skill

当文档需要与代码变更同步时使用。目标是保持文档准确并与实现对齐，不过度文档化。

---

## 工作流程

### 1. 确定影响范围

基于 git diff 和代码评审结论：
- 列出受影响的表/模型
- 列出受影响的接口和端点
- 列出受影响的 frontend 组件
- 列出受影响的业务流

### 2. 识别需更新的文档

基于影响范围：
- 架构文档
- API 文档
- 数据结构文档
- UML 图（时序图、类图）
- README 或索引文件

### 3. 更新文档

每个受影响的文档：
- 仅更新与变更相关的节
- 保持与代码术语一致
- 添加更新日期
- 保留无关节

### 4. 更新 UML 图

若业务流变更：
- 更新时序图
- 更新类图
- 若支持使用 Mermaid 格式
- 清晰标记参与者和步骤

### 5. 更新索引

若添加了新文档：
- 更新 README 索引
- 更新导航
- 添加「最后更新」日期

### 6. 验证一致性

- 文档匹配代码行为
- 无矛盾信息
- 术语一致

---

## 输出格式

```text
更新的文档:
- doc/path/file.md (原因)

更新的 UML 图:
- diagram/type (原因)

同步状态:
- 所有相关文档已更新
- 未发现矛盾
```

---

## Guardrails

- 仅更新与当前变更相关的部分
- 不为小变更重写整个文档
- 术语必须与代码完全匹配
- 在修改的节上标记更新日期
- 中文优先用于文档内容，技术术语可用英文