# AI Coding Agent Project Policy - Index

**Version:** 2.0
**Last Updated:** 2025-10-19
**Applies To:** All AI coding agents and software engineers

---

## 📌 Quick Start for AI Agents

This is the **main entry point** for understanding the project workflow. The complete policy has been modularized for efficient token usage. Load specific sections as needed.

---

## 🎯 Core Principles (Must Read First)

**Task-Driven Development:**
- ❌ NO code changes without an agreed-upon task
- ✅ Every change must have explicit authorization

**PBI Association:**
- ❌ NO task creation without an associated PBI
- ✅ All work traces back to a Product Backlog Item

**User Authority:**
- ✅ User is the sole decider for scope and design
- ✅ User retains responsibility for all code changes

**Status Synchronization:**
- ✅ Status in task index MUST match status in task file
- ✅ Update BOTH locations immediately on status change

**File Creation:**
- ❌ DO NOT create files outside defined structures
- ✅ Get explicit User confirmation before creating standalone files

---

## 📚 Detailed Documentation Sections

### Section 1: Fundamental Principles
**File:** `./docs/sections/1-fundamentals.md`

**When to reference:** Before starting any work, when uncertain about workflow rules

**Contains:**
- Core principles (Task-Driven, PBI Association, etc.)
- AI Agent automation rules
- PRD alignment requirements
- Change management rules
- Scope limitations and validation
- External package research guidelines

**Key topics:** DRY principle, constants usage, technical documentation requirements

---

### Section 2: PBI Management
**File:** `./docs/sections/2-pbi-management.md`

**When to reference:** Creating/managing Product Backlog Items, PBI state transitions

**Contains:**
- PBI workflow states and definitions
- All PBI state transitions (8 transitions)
- PBI history logging
- Backlog document structure
- PBI detail document structure

**Key workflows:** Creating PBI, Approving, Starting Implementation, Review, Completion

---

### Section 3: Task Management
**File:** `./docs/sections/3-task-management.md`

**When to reference:** Creating/managing tasks, task state transitions, daily work

**Contains:**
- Task workflow states and definitions
- All task state transitions (8 transitions)
- Task status synchronization rules
- Task concurrency limits
- Task history logging
- Version control for task completion
- Task validation rules

**Key workflows:** Approving task, Starting work, Submitting for review, Blocking/Unblocking

---

### Section 4: Testing Strategy
**File:** `./docs/sections/4-testing-strategy.md`

**When to reference:** Writing test plans, implementing tests, before marking tasks as Done

**Contains:**
- Testing principles (Risk-based, Test Pyramid)
- Test scoping by type (Unit, Integration, E2E)
- Test plan proportionality
- PBI-level vs Task-level testing
- E2E CoS test requirements
- Test implementation guidelines

**Key topics:** Mocking strategy, test plan detail levels, test distribution

---

### Section 5: Quick Reference
**File:** `./docs/sections/5-quick-reference.md`

**When to reference:** Daily workflow, checklists, common patterns, troubleshooting

**Contains:**
- Common workflow patterns (6 patterns)
- Decision trees
- Validation checklists
- Common pitfalls to avoid
- Best practices summary
- Git commands and templates

**Key patterns:** Starting new PBI, Working on task, Handling blocked tasks, Creating documentation

---

## 🚀 Quick Decision Guide for AI Agents

### "What should I do right now?"

```
1. Do I have a specific task to work on?
   ├─ NO  → Wait for User to assign task or approve PBI
   └─ YES → Continue to step 2

2. Is the task status "Agreed"?
   ├─ NO  → Cannot start. Wait for User approval
   └─ YES → Continue to step 3

3. Does status match in task file AND index?
   ├─ NO  → STOP. Report mismatch to User
   └─ YES → Continue to step 4

4. Are there other InProgress tasks for this PBI?
   ├─ YES → STOP. Wait for them to complete
   └─ NO  → Continue to step 5

5. All checks passed!
   └─ Follow workflow in Section 3 (Task Management)
```

### "Where do I find information about...?"

| Topic | Section | File |
|-------|---------|------|
| Can I make this change? | 1 | `./docs/sections/1-fundamentals.md` |
| How to create a PBI? | 2 | `./docs/sections/2-pbi-management.md` |
| How to start a task? | 3 | `./docs/sections/3-task-management.md` |
| What test plan do I need? | 4 | `./docs/sections/4-testing-strategy.md` |
| Step-by-step workflow? | 5 | `./docs/sections/5-quick-reference.md` |
| Validation checklist? | 5 | `./docs/sections/5-quick-reference.md` |
| Common pitfalls? | 5 | `./docs/sections/5-quick-reference.md` |

---

## ⚠️ Critical Rules (Never Violate)

These rules are MANDATORY at all times:

1. **NO code changes without an agreed task**
   - Every change must be authorized
   - If User requests change without task → discuss first

2. **NO task creation without a PBI**
   - All tasks must belong to a Product Backlog Item
   - Orphaned tasks are prohibited

3. **ALWAYS update both locations for status**
   - Task file AND task index must match
   - Update atomically in same commit

4. **ONLY ONE InProgress task per PBI**
   - Exception only with explicit User approval
   - Document approval in history

5. **NEVER create files without User confirmation**
   - Only create files within defined structures
   - Get explicit approval for standalone files

6. **ALWAYS verify before acting**
   - Check status matches in both locations
   - Validate transitions are allowed
   - Confirm no conflicts exist

---

## 📖 How to Use This Documentation

### For Initial Setup (First Time)
1. Read this index file completely
2. Read Section 1 (Fundamentals) - understand core principles
3. Skim Section 5 (Quick Reference) - know what's available
4. Reference Sections 2, 3, 4 as needed during work

### For Daily Work
1. Use "Quick Decision Guide" above
2. Reference specific sections when needed
3. Use checklists from Section 5 before key actions
4. Consult decision trees when uncertain

### For Troubleshooting
1. Check Section 5 "Common Pitfalls"
2. Use validation checklists
3. Review relevant state transition rules
4. Ask User if still unclear

---

## 📞 Getting Help

**When uncertain:**
- ✅ STOP work immediately
- ✅ Consult relevant section
- ✅ If still unclear, ASK User
- ❌ NEVER assume or guess

**When you find errors:**
- ✅ Report to User with details
- ✅ Provide recommended fix
- ✅ Wait for User guidance

---

## 🔄 Document Updates

This documentation is versioned and maintained. Always reference the most current version.

**Current Version:** 2.0
**Major Changes in v2.0:**
- Modularized structure for efficient token usage
- Added quick decision guide
- Enhanced reference system
- Separated concerns into focused sections

---

**Next Step:** Start with [Section 1: Fundamentals](./docs/sections/1-fundamentals.md) to understand core principles.
