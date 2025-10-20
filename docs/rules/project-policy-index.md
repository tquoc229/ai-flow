# AI Coding Agent Project Policy - Index

**Version:** 2.1 (Modular)
**Last Updated:** 2025-10-20
**Applies To:** All AI coding agents and Vietnamese software engineers

---

## Overview

This is the **modular version** of the AI Coding Agent Project Policy, split into focused sections for easier navigation and maintenance. This policy provides a **single, authoritative, machine-readable source of truth** for AI coding agents and humans, ensuring all work is governed by clear, unambiguous rules and workflows.

---

## Core Principles (Quick Summary)

### Task-Driven Development
- **NO code changes** without an agreed-upon task
- Every change must have explicit authorization
- All work traces back to a Product Backlog Item (PBI)

### User Authority
- User is the **sole decider** for scope and design
- User retains responsibility for all code changes (even if AI implements)

### Task Status Management (Single Source of Truth)
- Task detail file is the **authoritative source** for status
- Task index (`tasks.md`) is **auto-generated** via pre-commit hook
- Update status ONLY in task file - index regenerates automatically

### Prohibited Actions
- **NO** changes outside explicit task scope
- **NO** file creation without User confirmation
- **NO** gold plating or scope creep

---

## Documentation Structure

This policy is organized into **5 focused sections** plus this index:

### [Section 1: Fundamentals](./sections/1-fundamentals.md)
**Core principles and rules governing all work**

**Contains:**
- 1.1 Actors (User and AI_Agent roles)
- 1.2 Architectural Compliance
- 2.1 Core Principles (Task-Driven Development, PBI Association, PRD Alignment)
- 2.2 AI Agent Automation Rules
- 2.3 PRD Alignment Check
- 2.4 Integrity and Sense Checking
- 2.5 Scope Limitations
- 2.6 Change Management Rules

**Use this section when:**
- Starting a new project
- Onboarding new team members
- Clarifying fundamental workflow rules
- Understanding AI Agent automation requirements

---

### [Section 2: PBI Management](./sections/2-pbi-management.md)
**Product Backlog Item lifecycle and workflows**

**Contains:**
- 3.1 Overview
- 3.2 Backlog Document Structure
- 3.3 PBI Workflow States
- 3.4 PBI State Transitions (8 transitions)
- 3.5 PBI History Logging
- 3.6 PBI Detail Document Structure

**Use this section when:**
- Creating a new PBI
- Understanding PBI workflow states
- Transitioning PBI through lifecycle
- Setting up PBI documentation structure

---

### [Section 3: Task Management](./sections/3-task-management.md)
**Task creation, execution, and tracking workflows**

**Contains:**
- 4.1 Overview
- 4.2 Task Document Structure
- 4.3 Task Workflow States
- 4.4 Task State Transitions (8 transitions)
- 4.5 Task Status Management (Single Source of Truth) ✨ **NEW**
- 4.6 Task Concurrency Limit
- 4.7 Task History Logging
- 4.8 Task Validation Rules
- 4.9 Task Obsolescence Criteria ✨ **NEW**
- 4.10 Version Control for Task Completion
- 4.11 Task Index File Structure

**Use this section when:**
- Creating or starting a task
- Understanding task workflow
- Managing task status (now automated!)
- Identifying obsolete tasks
- Managing task concurrency
- Setting up task documentation

---

### [Section 4: Testing Strategy](./sections/4-testing-strategy.md)
**Testing principles, scoping, and documentation**

**Contains:**
- 5.1 Overview
- 5.2 Testing Principles
- 5.3 Test Scoping by Type (Unit, Integration, E2E)
- 5.4 Test Plan Documentation Strategy
- 5.5 Test Plan Requirements
- 5.6 Test Distribution Strategy
- 5.7 Test Implementation Guidelines
- 5.8 Test Failure Protocol (Autonomous Recovery) ✨ **NEW**

**Use this section when:**
- Designing test plans
- Determining appropriate test coverage
- Understanding test pyramid strategy
- Implementing unit, integration, or E2E tests
- Handling test failures autonomously

---

### [Section 5: Quick Reference](./sections/5-quick-reference.md)
**Common patterns, workflows, checklists, and best practices**

**Contains:**
- Common Workflow Patterns (6 patterns)
- Common Commands and Patterns
- Decision Trees
- Validation Checklists
- Common Pitfalls to Avoid
- Best Practices Summary

**Use this section when:**
- Need quick guidance for common tasks
- Making workflow decisions
- Validating before actions
- Troubleshooting common issues

---

## Quick Decision Guide for AI Agents

### Before Starting Any Work

```
1. Is there an associated PBI?
   NO → Create PBI first
   YES → Continue

2. Is PBI plan approved?
   NO → Create plan, get User approval first
   YES → Continue

3. Are tasks created and auto-approved?
   NO → Decompose plan into tasks (auto-set to "Agreed")
   YES → Continue

4. Is task status "Agreed" in task file?
   NO → STOP and report issue
   YES → Continue

5. Are there other InProgress tasks for this PBI?
   YES → STOP, only one task InProgress allowed
   NO → Continue

6. ✅ Proceed with work
```

### During Implementation

```
1. Make ONLY changes within task scope
2. Reference task ID in ALL commits
3. Update documentation as you go
4. Run tests continuously
5. If scope expands → STOP, create new task
```

### Before Submitting for Review

```
1. All requirements implemented? ✅
2. All tests passing? ✅
3. Files modified documented? ✅
4. Status updated in task file? ✅
5. History logged? ✅
6. Committed task file (index auto-generates)? ✅
```

---

## Critical Rules Summary

### Absolutely Mandatory

1. ✅ **NO code changes** without agreed task
2. ✅ **NO task creation** without associated PBI
3. ✅ **ALWAYS** update task status in task file (index auto-generated)
4. ✅ **ONLY ONE** task InProgress per PBI at a time
5. ✅ **ALL** status changes must be logged in history
6. ✅ **NO** file creation without User confirmation
7. ✅ **NO** scope creep - stay within task boundaries
8. ✅ **EVALUATE** remaining tasks for obsolescence after each completion

### When to STOP and Report to User

- ❌ Multiple InProgress tasks found for same PBI
- ❌ Task not in "Agreed" state
- ❌ Scope expanding beyond task definition
- ❌ Tests failing (use Test Failure Protocol - see Section 4)
- ❌ Test expectation mismatch (escalate immediately)
- ❌ Dependencies unavailable (mark as Blocked)

---

## How to Use This Documentation

### For AI Agents

1. **Start with Section 1 (Fundamentals)** to understand core principles
2. **Reference Section 2 (PBI Management)** when working with PBIs
3. **Reference Section 3 (Task Management)** when working with tasks
4. **Reference Section 4 (Testing Strategy)** when creating test plans
5. **Use Section 5 (Quick Reference)** for day-to-day guidance

### For Human Users

1. **Review Section 1** to understand the system philosophy
2. **Bookmark Section 5** for quick workflow references
3. **Reference other sections** as needed for specific workflows

### For Onboarding

1. Read Section 1 completely
2. Skim Section 2 and Section 3 for workflow overview
3. Review Section 5 for practical examples
4. Deep-dive into specific sections as needed

---

## Navigation

- [Section 1: Fundamentals →](./sections/1-fundamentals.md)
- [Section 2: PBI Management →](./sections/2-pbi-management.md)
- [Section 3: Task Management →](./sections/3-task-management.md)
- [Section 4: Testing Strategy →](./sections/4-testing-strategy.md)
- [Section 5: Quick Reference →](./sections/5-quick-reference.md)

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 2.1 | 2025-10-20 | Added 3 major improvements: Single Source of Truth for task status, Test Failure Protocol, Task Obsolescence Criteria |
| 2.0 | 2025-10-19 | Split into modular structure for easier navigation |
| 1.0 | 2025-10-19 | Initial monolithic version |

---

**End of Index**
