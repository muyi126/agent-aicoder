# Rules

Rules define reusable behavioral constraints for specific scenarios. They can be referenced by Skills and applied during execution.

---

## Available Rules

| Rule | Description | When to Use |
|------|-------------|-------------|
| `agent-mode.md` | Work mode specification (Research → Innovation → Plan → Execute → Review) | When user specifies a mode or transitions between phases |
| `architecture-guide.md` | Architecture design guide (layering, module division, naming) | When designing service architecture or understanding project structure |
| `channel-dev-rule.md` | Third-party channel integration standards | When integrating with third-party APIs |
| `generate-code.md` | Code generation standards based on technical specifications | When generating code from solution/task documents |
| `microservice-dev-standards.md` | Development standards (logging, exception handling, layered responsibilities) | When writing code or reviewing implementations |
| `microservice-standards-core.md` | Microservice core standards (SOLID, state machines, idempotency) | When designing services or defining service boundaries |
| `microservice-understanding-rule.md` | Microservice deep scan protocol for architecture analysis | When analyzing a microservice or generating service documentation |
| `project-analyze.md` | Project analysis rules for systematic architecture analysis | When analyzing a project or generating architecture documentation |

---

## Rule Format

Each rule should have frontmatter:
```yaml
---
description: Brief description of when to use this rule
alwaysApply: false  # true if rule should always be active
---
```

---

## Usage

Rules are referenced in Skills using:
```
@rules/rule-name.md
```

For example, in a Skill:
```
**Rules**: @rules/architecture-guide.md, @rules/code-review.md
```

---

## Guidelines

- Rules should be focused and reusable across multiple skills
- Rules define constraints, not detailed implementation steps
- Rules should reference other rules when appropriate
- Keep rules technology-agnostic unless specifically scoped