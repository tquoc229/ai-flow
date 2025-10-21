# MyFlow Project Policy - Index

<critical>This is the entry point to MyFlow policy documentation. Read this first before starting work.</critical>
<critical>Version: 3.0 (BMAD-Inspired) | Last Updated: 2025-10-21</critical>

---

## Quick Start for AI Agents

**Before doing ANY work:**

1. ✅ Check you have a PBI → If NO, use `*create-pbi`
2. ✅ Check PBI plan approved → If NO, wait for user approval
3. ✅ Check tasks created → If NO, use `*decompose-tasks`
4. ✅ Check task status = "Agreed" → If NO, STOP
5. ✅ Check NO other InProgress tasks → If YES, STOP

**Ready?** Use `*execute-task` to implement.

---

## Core Principles

### Task-Driven Development
- **NO code changes without an agreed task**
- Every change must have explicit authorization
- All work traces back to a PBI

### User Authority
- User is the sole decider for scope and design
- User retains responsibility for all code changes

### Single Source of Truth
- Task detail file is authoritative for status
- Task index (`tasks.md`) is auto-generated
- Never edit task index manually

### Prohibited Actions
- ❌ NO changes outside task scope
- ❌ NO file creation without user confirmation
- ❌ NO gold plating or scope creep

---

## Critical Rules (MANDATORY)

1. **NO code changes without an agreed task**
2. **NO task creation without an associated PBI**
3. **ALWAYS update task status in the task file**
4. **ONLY ONE task InProgress per PBI at a time**
5. **ALL status changes must be logged in history**
6. **NO file creation without user confirmation**
7. **NO scope creep; stay within task boundaries**
8. **EVALUATE remaining tasks for obsolescence after each completion**

### When to HALT and Report

Stop immediately when:
- Multiple InProgress tasks found for same PBI
- Task is not in "Agreed" state
- Scope is expanding beyond task definition
- Tests are failing (in strict mode)
- Test expectation mismatch
- Dependencies unavailable (mark as "Blocked")

---

## Documentation Structure

MyFlow policy is organized into **5 focused sections** + this index.

### [Section 1: Fundamentals](./sections/1-fundamentals.md)

**Content:**
- Core principles and roles
- Architectural compliance
- Automation rules
- Change management

**When to Load:**
- Onboarding
- Clarifying fundamental rules
- Understanding MyFlow philosophy

**Lines:** ~150 lines

---

### [Section 2: PBI Management](./sections/2-pbi-management.md)

**Content:**
- PBI lifecycle and states
- Valid state transitions
- PBI document structure (backlog.md, PRD)
- Legacy Discovery rules
- Validation criteria

**When to Load:**
- Creating or managing PBIs
- Understanding PBI workflows
- Reference for PBI states

**Lines:** ~223 lines

**Related Workflows:**
- `workflows/create-pbi/` - Create PBI with Legacy Discovery
- `workflows/decompose-tasks/` - Break down PBI into tasks

---

### [Section 3: Task Management](./sections/3-task-management.md)

**Content:**
- Task lifecycle and states
- Valid state transitions
- Task document structure
- Concurrency limit (max 1 InProgress)
- Task obsolescence criteria
- Version control guidelines

**When to Load:**
- Creating or executing tasks
- Understanding task workflows
- Reference for task states

**Lines:** ~290 lines

**Related Workflows:**
- `workflows/decompose-tasks/` - Create tasks from PBI
- `workflows/execute-task/` - Implement task (Agreed → InReview)

---

### [Section 4: Testing Strategy](./sections/4-testing-strategy.md)

**Content:**
- Test proportionality matrix
- Coverage requirements
- Test types (Unit, Integration, E2E)
- Testing principles (pyramid, risk-based)
- Test execution modes
- Mocking strategy

**When to Load:**
- Implementing tasks with tests
- Designing test plans
- Understanding test requirements

**Lines:** ~332 lines

**Related Workflows:**
- `workflows/execute-task/` - Includes test execution (steps 6-8)

---

### [Section 5: Quick Reference](./sections/5-quick-reference.md)

**Content:**
- Common workflow patterns
- Git commands and patterns
- Decision trees
- Validation checklists
- Common pitfalls
- Best practices

**When to Load:**
- Day-to-day quick reference
- Troubleshooting
- Workflow reminders

**Lines:** ~568 lines

---

## Workflows Directory

For **step-by-step execution instructions**, see `workflows/` directory.

Each workflow is self-contained with:
- `workflow.yaml` - Configuration and variables
- `instructions.md` - XML-based execution guide
- `checklist.md` - Validation checklist (if applicable)
- `README.md` - Documentation

### Available Workflows

| Workflow | Purpose | Command |
|----------|---------|---------|
| **create-pbi** | Create PBI with Legacy Discovery and PRD | `*create-pbi` |
| **decompose-tasks** | Break down approved PBI into tasks | `*decompose-tasks` |
| **execute-task** | Implement task (Agreed → InReview) | `*execute-task` |

**See:** `workflows/README.md` for complete workflow documentation.

---

## Policy vs Workflows

**Understanding the separation:**

| docs/rules/ | workflows/ |
|-------------|------------|
| **WHAT & WHY** | **HOW** |
| Policy reference | Execution guide |
| States, rules, principles | Step-by-step instructions |
| Quick lookup | Detailed workflows |
| ~1000 lines total | 200-400 lines per workflow |

**When to Load:**
- **Policy sections:** For reference, understanding rules, state definitions
- **Workflows:** When actually executing that workflow

**Benefits:**
- No duplication
- Optimized context loading (63% reduction)
- Clear separation of concerns

---

## Pre-Work Checklist (AI Agents)

Before starting ANY work:

```
[ ] Have associated PBI
[ ] PBI plan approved (status: ReadyForTasks or InProgress)
[ ] Tasks created and auto-approved (status: Agreed)
[ ] Task status confirmed in task detail file
[ ] No other InProgress tasks for this PBI (max_concurrent: 1)
[ ] All required files accessible
```

If ALL checked: Proceed with `*execute-task`

If ANY unchecked: STOP and resolve issue first

---

## Pre-Submission Checklist

Before marking task as InReview:

```
[ ] All task requirements implemented
[ ] All tests passing
[ ] Files Modified section accurate
[ ] Task status updated in task file
[ ] Task history logged with timestamps
[ ] Implementation notes appended
[ ] Validation checklist passed (95%+)
```

---

## Context Loading Strategy

**Startup (ALWAYS load):**
1. `AGENTS.md` - Primary directive
2. `ai-flow-config.yaml` - Configuration
3. `project-policy-index.md` (this file) - Overview

**On-Demand (load when needed):**
- Section 2 - When working with PBIs
- Section 3 - When working with tasks
- Section 4 - When implementing tests
- Section 5 - For quick reference
- Workflow files - When executing that workflow

**Token Efficiency:**
- Policy index: ~200 lines
- Specific section: 150-332 lines
- Workflow: 200-400 lines
- **Total context: 550-930 lines** (vs 3000+ monolithic)

---

## Navigation Quick Links

### Policy Sections
- [Section 1: Fundamentals](./sections/1-fundamentals.md)
- [Section 2: PBI Management](./sections/2-pbi-management.md)
- [Section 3: Task Management](./sections/3-task-management.md)
- [Section 4: Testing Strategy](./sections/4-testing-strategy.md)
- [Section 5: Quick Reference](./sections/5-quick-reference.md)

### Workflows
- [Workflows Index](../../workflows/README.md)
- [Create PBI](../../workflows/create-pbi/)
- [Decompose Tasks](../../workflows/decompose-tasks/)
- [Execute Task](../../workflows/execute-task/)

### Configuration
- [ai-flow-config.yaml](../../ai-flow-config.yaml) - Central configuration
- [AGENTS.md](../../AGENTS.md) - AI agent directive

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 3.0 | 2025-10-21 | BMAD-inspired refactor. Separated policy from workflows. Directory-based workflow organization. Context optimization. |
| 2.1 | 2025-10-20 | Added Single Source of Truth for task status, Test Failure Protocol, Task Obsolescence Criteria. |
| 2.0 | 2025-10-19 | Split into modular structure for easier navigation. |
| 1.0 | 2025-10-19 | Initial monolithic version. |

---

## Getting Help

### For AI Agents

**If uncertain:**
1. Check this index for overview
2. Load relevant policy section for details
3. Load workflow for execution steps
4. If still unclear: STOP and ASK user

**Common Questions:**
- "What state should this be?" → Check Section 2 or 3
- "How do I execute this?" → Load workflow
- "What are the rules?" → This index + relevant section

### For Humans

**Getting Started:**
1. Read this index completely
2. Skim Section 1 (Fundamentals)
3. Dive into specific sections as needed
4. Use Section 5 for daily reference

**Finding Information:**
- Use this index for navigation
- Policy sections for rules and states
- Workflows for step-by-step guides

---

**Welcome to MyFlow v3.0!** 🚀

Start here, load what you need, work efficiently.
