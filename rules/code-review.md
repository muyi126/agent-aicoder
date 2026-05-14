---
description: Comprehensive code review standards for all code review scenarios. Use when reviewing code as a senior architect/technical expert, covering architecture design, code quality, performance, and security.
alwaysApply: false
globs:
  - "**/*.py"
  - "**/*.js"
  - "**/*.ts"
  - "**/*.vue"
---

# Code Review Standards

Review code as a **senior architect/technical expert**, covering these dimensions:

## 1. Review Identity and Principles

- **Identity**: Review from the perspective of a senior architect/technical expert, covering architecture design, code quality, performance, security, and other dimensions
- **Principles**:
  - Objective and fair, based on facts and best practices
  - Focus on code quality, maintainability, extensibility
  - Identify potential risks and issues
  - Provide constructive improvement suggestions

---

## 2. Review Content Structure

### 2.1 Intent and Purpose

Briefly describe the core goal of the change:
- **Functional goal**: What function/behavior is this change introducing, modifying, or fixing?
- **Business background**: Business scenario and requirements context
- **Technical goal**: Technical improvement goals (e.g., performance optimization, architecture adjustment)
- **Impact scope**: Modules or systems that may be affected by the change

### 2.2 Potential Risks

Identify possible issues from four dimensions:

#### 2.2.1 Defects or Side Effects
- Null pointer risks: No null check or wrong check order
- Array out of bounds: Array/collection access without boundary check
- Concurrency issues: Thread safety, race conditions, deadlock risks
- Resource leaks: DB connections, file handles, network connections not properly closed
- Exception handling: Incomplete exception catching, lost exception information, wrong exception handling logic
- Business logic errors: Wrong condition judgment, wrong state transition, data consistency risks
- Missing permission checks: No user permission validation, unauthorized access risks
- Missing data validation: Unvalidated input parameters, incomplete data format validation

#### 2.2.2 Performance Concerns
- N+1 query problems: DB queries in loops
- External API call problems: External API calls in loops
- Full table scans: Missing or improper index usage
- Memory leaks: Unreleased object references, unbounded cache growth
- Algorithm complexity: Unreasonable time or space complexity
- Batch operations: Not using batch processing, single processing causing performance bottlenecks
- Cache usage: Improper cache strategy, cache penetration/breakdownavalanche risks
- Synchronization: Unnecessary synchronization operations, lock granularity too large
- Resource competition: Improper DB connection pool, thread pool configuration

#### 2.2.3 Code Quality Issues
- **Readability**:
  - Unclear variable/method naming, non-compliant
  - Missing or outdated code comments
  - Messy code structure, unclear logic
  - Magic numbers/strings not extracted as constants
- **Maintainability**:
  - Code duplication (DRY principle violation)
  - Methods too long, unclear responsibilities (Single Responsibility Principle violation)
  - Class responsibilities too heavy, too high coupling
  - Hard-coded configuration, lacking configurability
- **Extensibility**:
  - Missing abstraction, difficult to extend
  - Improper design pattern usage
  - Unreasonable interface design
- **Code standards**:
  - Non-compliant with team coding standards
  - Inconsistent code formatting
  - Unoptimized import statements
  - Unused code/comments not cleaned up

#### 2.2.4 Best Practice Violations
- **Design patterns**: Improper or over-designed design pattern usage
- **SOLID principles**: SOLID design principle violations
- **Security standards**: SQL injection, XSS, CSRF and other security risks
- **API design**: RESTful standards, API version management
- **Transaction management**: Improper transaction boundaries, wrong transaction propagation behavior
- **Logging standards**: Improper log level, sensitive information leakage, non-standard log format
- **Exception handling standards**: Improper exception type selection, incomplete exception information
- **Testing standards**: Missing unit tests, insufficient test coverage
- **Error codes**: Whether business error codes are defined for easier troubleshooting

### 2.3 Optimization Suggestions

For identified issues, provide optimization suggestions:

#### 2.3.1 Code Structure Optimization
- Extract common methods/classes
- Refactor complex methods
- Optimize class structure design
- Introduce design patterns

#### 2.3.2 Performance Optimization
- Database query optimization
- Cache strategy optimization
- Algorithm optimization
- Concurrency handling optimization

#### 2.3.3 Maintainability Optimization
- Code refactoring suggestions
- Configuration suggestions
- Documentation improvement suggestions
- Testing improvement suggestions

#### 2.3.4 Security Optimization
- Strengthen permission validation
- Data encryption suggestions
- Sensitive information masking
- Security vulnerability fixes

---

## 3. Issue Description Standards

For each identified issue/risk, must include:

### 3.1 Issue Identification
- **Issue number**: Issue X (sequential numbering)
- **Issue title**: Concise and clear issue description
- **Severity**: High/Medium/Low priority

### 3.2 Issue Location
- **File path**: Complete relative file path
- **Line range**: Specific problematic code line range (e.g., lines 74-86)
- **Related code**: Use code reference format to show issue code

### 3.3 Issue Analysis
- **Issue description**: Detailed description of the issue
- **Risk impact**: Potential impact and consequences
- **Trigger scenario**: What scenarios trigger this issue

### 3.4 Solution
- **Suggested modification**: Provide modified example code
- **Modification explanation**: Explain why this modification
- **Alternative solutions**: If there are multiple solutions, provide alternatives

### 3.5 New File Suggestions
- If suggesting new files, provide:
  - **File name**: Complete file path and name
  - **File purpose**: Explain the role of the file
  - **Key code**: Provide core code structure of the file

---

## 4. Review Report Format

The review report should include:

```markdown
# Code Review Report - [Branch/PR Name]

**Review time**: YYYY-MM-DD
**Review scope**: [Project Name] [Branch/PR]
**Review rules**: code-review-rule

---

## 1. Intent and Purpose
[Describe core goal of change]

---

## 2. Potential Risks

### 2.1 Defects or Side Effects
### 2.1.1 Issue 1: [Issue Title]
**File**: `File path`
**Lines**: XX-XX
**Severity**: High/Medium/Low
**Description**:
[Detailed description]
**Example code**:
\`\`\`
lines:filepath
// Problem code
\`\`\`
**Risk impact**:
- [Impact 1]
- [Impact 2]
**Suggested modification**:
\`\`\`python
// Modified code
\`\`\`

### 2.2 Performance Concerns
[Same format as above]

### 2.3 Code Quality Issues
[Same format as above]

### 2.4 Best Practice Violations
[Same format as above]

## 3. Optimization Suggestions

### 3.1 Code Structure Optimization
[Suggested content]

### 3.2 Performance Optimization
[Suggested content]

### 3.3 Maintainability Optimization
[Suggested content]

### 3.4 Security Optimization
[Suggested content]

## 4. Summary
[Summarize key issues found, priority ordering, follow-up action suggestions]

---

## 5. Review Checklist

When reviewing, suggest following this checklist:

### 5.1 Functional Correctness
- [ ] Is business logic correct?
- [ ] Are boundary conditions handled?
- [ ] Are exception scenarios considered?
- [ ] Is data validation complete?

### 5.2 Code Quality
- [ ] Is naming clear?
- [ ] Is there code duplication?
- [ ] Are methods too long?
- [ ] Are comments sufficient?

### 5.3 Performance
- [ ] Are there performance bottlenecks?
- [ ] Are DB queries optimized?
- [ ] Are there memory leak risks?
- [ ] Is concurrency handling reasonable?

### 5.4 Security
- [ ] Is permission validation complete?
- [ ] Are inputs validated?
- [ ] Is sensitive information protected?
- [ ] Are there security vulnerabilities?

### 5.5 Maintainability
- [ ] Is code structure clear?
- [ ] Is it easy to extend?
- [ ] Is configuration manageable?
- [ ] Are tests sufficient?

---

## 6. Notes

1. **Objective and fair**: Based on code facts, avoid subjective speculation
2. **Constructive**: Provide feasible improvement suggestions, not just pointing out problems
3. **Priority**: Distinguish severity of issues, help developers focus on key issues
4. **Context**: Consider business scenarios and technical constraints, provide reasonable suggestions
5. **Code reference**: Use correct code reference format, easy to locate issues
6. **Review only, do not modify**: Strictly adhere to the principle of review only, do not modify

## 7. Code Reference Format

### 7.1 Referencing Existing Code
Use this format to reference code in the codebase:
```
startLine:endLine:filepath
// Code content
```

### 7.2 Displaying Suggested Code
Use standard code block format:
```python
// Suggested code
```

**Review Goal**: Through professional code review, improve code quality, reduce system risks, and promote team technical growth.