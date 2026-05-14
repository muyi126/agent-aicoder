---
description: Project analysis rules for systematic project architecture and code structure analysis. Use when the user asks to analyze a project or generate architecture documentation.
alwaysApply: false
---

# Project Analysis Rules

## 1. Applicable Scenarios

When the user initiates requests related to "project analysis", this rule must be enabled:
- Using command: `init.md <project>`, require output of project analysis document
- User uses natural language:
  - "Analyze this project"
  - "Help me understand this project's architecture"
  - "Generate architecture analysis document for this project"

---

## 2. Role and General Principles

- **Role**: Senior architect and code audit expert
- **Overall goal**: On the premise of **not modifying any code**, perform stable, repeatable, structured analysis of the project according to fixed processes
- **Priority principle**:
  1. **First execute rules, then write conclusions**
  2. Must complete "interface exhaustive + domain classification + missing check" three steps before generating final analysis report
  3. For discovered missing items, must explicitly mark the source rule and reason in subsequent responses

---

## 3. Execution Process (Staged)

> △ Unless the user explicitly requests interruption, the following phases must be executed in order, no skipping allowed.

### Phase 0: Rule Awareness and Execution Plan (Must Output)

Before starting any scanning or analysis, must briefly output in the response:
1. **List of rules used this time** (at least include `microservice-understanding-rule` and `project-analyze`)
2. **Key constraint restatement** (briefly explain 3-5 key rules that must be followed this time in your own words), at least including:
   - Must list all interface files
   - Must group by business domain
   - Must check if key domains are covered
   - Must output a "Rule Execution Status" section before the final report
3. **Execution plan** (summarize upcoming phases in 3-5 bullet points: full indexing → domain classification → missing check → report)

### Phase 1: Interface Full Indexing

1. Use `list_dir` + `glob` or `grep` to, for the current project:
   - List **all interface files** (usually `*Controller.py`, `*Routes.py`, `*Service.py`)
   - List all API route definitions
2. Output in *table or list* format in the response:
   - Interface file list
   - API route list (if exists)

> Prohibited: Only do precise search on single interface keyword without first enumerating file names.

### Phase 2: Business Domain Classification (Categorize)

1. Based on Phase 1's interface list, tag interfaces by **business domain**, recommended but not limited to the following preset domains:
   - `Core Business`
   - `User Management`
   - `Data Management`
   - `Configuration Management`
   - `Operations / Admin`
   - `Other auxiliary domains (e.g., monitoring, tracing, testing)`
2. Output a **business domain - interface list matrix** in the response. For each domain, list at least file names.
3. If certain preset domains do not exist in the current project, must explicitly mark as *domain not found in this project*.

> Prohibited: Only deeply explore individual attention domains without doing complete domain classification for all interfaces.

### Phase 3: Missing Check & Verification (Check for Missing)

1. Combine with thinking guidance from `microservice-understanding-rule`, **explicitly check** if the following domains are already in the matrix:
   - Core business functions
   - User management
   - Data management
   - Configuration management
   - Operations management
2. Output a **domain coverage checklist** in the response. Each row contains:
   - Domain name
   - Status: `Exists / Not found / Not applicable`
   - If status is `Exists`, briefly list 1-2 representative interface names
3. If a domain exists in the code but was not included in the previous round of classification:
   - Must immediately update business domain matrix
   - Ensure in the subsequent final analysis document that the domain has an independent section

### Phase 4: Analysis Report Generation

1. Generate project analysis report based on the template provided by `microservice-understanding-rule`
2. Must include the following parts:
   - Architecture overview
   - Interface panorama (grouped by business domain, covering all domains from Phase 2)
   - Key business rules
   - Technical implementation details
   - Dependency relationships
   - Exception and stability
3. **Must add a "Rule Execution Status" section at the beginning of the report** (see next section)

---

## 4. "Rule Execution Status" Mandatory Paragraph

When generating any project analysis report (including intermediate responses and final documents), must add the following structured paragraph at the beginning:

### Rule Execution Status (Required)

- **Checklist-1 Full indexing executed**: Yes / No
  - Explanation: Whether all interface file names have been listed
- **Checklist-2 Business domain classification completed**: Yes / No
  - Explanation: Whether each interface has been tagged with business domain and matrix output
- **Checklist-3 Key domain coverage check completed**: Yes / No
  - Explanation: Whether key domains have been checked for existence and coverage table output
- **Checklist-4 Report based on latest matrix**: Yes / No
  - Explanation: Whether the interface panorama in the report is confirmed to be consistent with the latest domain matrix
- **Discovered known missing items / corrections**: Briefly list (if none, write "none")

> If any Checklist item is "No", AI must not claim the analysis is complete; should continue to execute the corresponding phase.

---

## 5. Rule Violation Handling (When Missing Found Later)

When pointed out by user in subsequent conversation that there are missing items:
1. Use format:
   - `⚠ Rule execution violation: [specific business domain name] not covered`
2. Clearly indicate:
   - Which rule was violated (reference number)
   - Direct cause of the omission
3. Give **self-restraint explanation**:
   - What specific actions will be added in Phase 1-3 in similar scenarios in the future
4. In the same response, based on this rule, fill in the missing parts (update interface domain matrix & report fragment)

---

## 6. Relationship with `microservice-understanding-rule`

- When the two rules are simultaneously applicable:
  - `microservice-understanding-rule` is responsible for defining **analysis dimensions and output structure**
  - `project-analyze` is responsible for constraining **execution process and rule awareness / missing check closed loop**
- If there are conflicts between the two, **the stricter and more specific constraint** takes precedence

---

## 7. Thinking Process

Before answering, you must:
1. **Breadth First**: First scan project structure, list all interfaces, ensure no interfaces are missed
2. **Categorize**: Categorize the files you see in your mind. Do not try to show all files in one list
3. **Check for Missing**: Ask yourself what other auxiliary functions exist besides core functions? Are there admin interfaces? Operations interfaces?
4. **Context Aware**: Combine with currently opened files, determine whether user concerns are macro architecture or specific implementation