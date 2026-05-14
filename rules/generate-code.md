---
description: Code generation standards, defining how to write code based on technical specifications. Use when generating or modifying code according to a solution document and task list.
alwaysApply: false
---

# Code Generation Standards

## 1. Input

- Technical solution (solution.md)
- Task list (task-list.md)
- Service name (service)
- Project code (service)
- Project understanding document: `@[service]-analyze.md`

---

## 2. Core Tasks

### 2.1 Document Analysis and Understanding Phase

Before writing code, complete the following analysis:

**Requirement matching check**:
- Deeply understand requirement documents and solution design documents, confirm complete consistency between `solution.md` and `task.md` in terms of functional points, input/output, and exception scenarios
- If contradictions are found, immediately mark and submit notes

**Code architecture understanding**:
- Deeply understand project analysis documents and existing code base layered structure, determine insertion position for new functionality
- List reusable utility classes, exception handling mechanisms, and public interfaces (e.g., `utils.java`, `ErrorCode` enum classes)

---

### 2.2 Code Generation Phase

If requirements and technical solution are clear, complete code writing work.

**Note**: After code output, compile. If compilation problems occur, fix them.

**Note**: After compilation passes, generate unit tests. Reference project's existing unit test style, use rule: `@Rule unit-test`

---

## 3. Requirements

1. **Code you generate must reference the current project's code style**
2. **If the project already has available methods, must consider reusing, extending, or method overloading existing methods to ensure minimum granularity changes and reduce duplicate code**

---

## 4. Output

Generate code and place it in the appropriate location in the code base.

---

## 5. Code Style Checklist

- [ ] Follow existing naming conventions
- [ ] Follow existing directory structure
- [ ] Follow existing error handling patterns
- [ ] Follow existing logging format
- [ ] Add unit tests with same style as existing tests
- [ ] No hardcoded values, use configuration
- [ ] Add appropriate comments for complex logic
- [ ] Use type hints

---

## 6. Verification Requirements

After code generation:
1. Run compilation check
2. If compilation fails, fix and recheck
3. Generate unit tests
4. Ensure all tests pass