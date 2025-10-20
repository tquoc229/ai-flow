# AI Agent System Instructions

**Version:** 2.1
**Last Updated:** 2025-10-20

---

## 🎯 Primary Directive

You are an AI coding agent working under a **strict task-driven workflow**.

**Your complete working instructions are located in the project policy documentation.**

---
## Project Docs
- [Project Overview / PDR](docs/project-overview.md)
- [Reusable Components](Reusable-Components.md)
- [Design Guideline](docs/DesignGuideline.md)
- [Api Docs](docs/api-docs.md)
- [Integration Guilde](docs/integration-guide.md)
- [Codebase Structure & Code Standards](docs/codebase-structure-architecture-code-standards.md)


## 📚 Required Reading - START HERE

**BEFORE doing ANY work, you MUST read:**

### 1. Main Policy Index
**File:** `docs/rules/project-policy-index.md`

This is your **primary reference**. It contains:
- ✅ Core principles you must follow
- ✅ Quick decision guide
- ✅ Links to all detailed sections
- ✅ Critical rules that NEVER can be violated

**Load this file FIRST** - it's only ~250 lines and gives you complete overview.

### 2. Load Sections On-Demand

When you need detailed workflow instructions, load the appropriate section:

| When you need to... | Load this section |
|---------------------|-------------------|
| Understand core principles | `docs/rules/sections/1-fundamentals.md` |
| Work with PBI | `docs/rules/sections/2-pbi-management.md` |
| Work with Task | `docs/rules/sections/3-task-management.md` |
| Write tests | `docs/rules/sections/4-testing-strategy.md` |
| Find checklists/patterns | `docs/rules/sections/5-quick-reference.md` |

---

## 🚫 Critical Rules - Never Violate

These are the ABSOLUTE rules from the policy:

1. **NO code changes** without an agreed-upon task
2. **NO task creation** without an associated PBI
3. **NO changes** outside explicit task scope
4. **NO file creation** without User confirmation
5. **ALWAYS** update task status (now uses Single Source of Truth - see below)
6. **ONLY ONE** task per PBI can be InProgress at a time

**Violating these rules is unacceptable.**

---

## ✨ Recent Workflow Improvements

### 1. Single Source of Truth for Task Status

**Old approach:** Update status in BOTH task file AND index (error-prone, synchronization issues)

**New approach:**
- ✅ Task detail files (`<PBI-ID>-<TASK-ID>.md`) are the **only authoritative source**
- ✅ Task index (`tasks.md`) is **auto-generated** via pre-commit hook
- ✅ Update status in ONE place only (task file)
- ✅ Impossible to have mismatches - index always derived from task files

**What this means for you:**
- Update status ONLY in the task detail file
- Do NOT manually edit `tasks.md` index files
- Pre-commit hook will regenerate index automatically

### 2. Test Failure Autonomous Recovery

**You now have clear protocols for handling test failures:**

**Category A - Environment Issues** (auto-retry with backoff):
- Database connection failures, port conflicts, memory issues
- Auto-retry: Immediate → 5s delay → 15s delay (3 attempts)
- Continue if any retry succeeds

**Category B - Code Defects** (auto-fix):
- Assertion failures, logic errors, implementation bugs
- Analyze error, fix implementation, re-run tests
- Max 2 fix attempts before escalating

**Category C - Test Expectation Wrong** (escalate):
- Requirements changed, test assumptions invalid
- STOP and escalate to User with analysis

**Details:** See Section 5.8 in `docs/rules/sections/4-testing-strategy.md`

### 3. Task Obsolescence Criteria

**You can now proactively identify tasks that became unnecessary:**

**4 Concrete Criteria:**
1. **Already Satisfied** - Previous work fulfilled >= 80% of task requirements
2. **Superseded** - Implementation approach diverged, making task irrelevant
3. **External Dependency** - Third-party package now provides >= 80% of functionality
4. **Requirements Changed** - User explicitly removed from scope

**What this means for you:**
- After completing each task, evaluate remaining tasks using these 4 criteria
- Flag tasks that match criteria as potentially OBSOLETE
- Ask User to confirm removal or modification
- Prevents wasted work on irrelevant tasks

**Details:** See Section 4.9 in `docs/rules/sections/3-task-management.md`

---

## 🔄 Task Auto-Approval Workflow

**Important:** This project uses **Task Auto-Approval** to reduce overhead:

### How It Works:

1. **User approves PBI plan** (not individual tasks)
2. **You automatically create all tasks** from the approved plan
3. **Tasks are auto-set to "Agreed" status** (skip "Proposed" state)
4. **You can immediately start work** on any task

### Why Auto-Approval?

- ✅ User already approved the overall plan at PBI level
- ✅ Tasks are derived directly from approved plan
- ✅ Eliminates need for User to click "approve" 20+ times
- ✅ User can still review/modify tasks before they start

### User Still Controls:

- ✅ Approves PBI plan (which includes task breakdown)
- ✅ Can modify task details before work starts
- ✅ Can reject specific tasks or add new ones
- ✅ Approves each task after completion (Review → Done)

**You do NOT need to ask User to approve each task individually.**

---

## 🚀 Quick Start

```
Step 1: Read docs/rules/project-policy-index.md
        ↓
Step 2: Use the "Quick Decision Guide" to determine what to do
        ↓
Step 3: Load the relevant section for detailed workflow
        ↓
Step 4: Follow the workflow EXACTLY as described
        ↓
Step 5: When uncertain, STOP and ASK the User
```

---

## 📖 Where to Find Answers

**Question:** "Can I do X?"
**Answer:** Check `docs/rules/project-policy-index.md` → Core Principles section

**Question:** "How do I work with PBI/Task?"
**Answer:** Load the corresponding section (2 or 3) from the index

**Question:** "What's the step-by-step for this workflow?"
**Answer:** Load Section 5 (Quick Reference) for patterns and checklists

**Question:** "What tests do I need?"
**Answer:** Load Section 4 (Testing Strategy)

---
## Development Rules

- use `context7` mcp tools for docs of plugins/packages
- use `chrome-devtools` mcp tools for run and test frontend
- use `psql` bash command to query database for debugging


## Pre-commit/Push Rules

- Run linting before commit
- Run tests before push (DO NOT ignore failed tests just to pass the build or github actions)
- Keep commits focused on the actual code changes
- DO NOT commit and push any confidential information (such as dotenv files, API keys, database credentials, etc.) to git repository!
- NEVER automatically add AI attribution signatures like: "🤖 Generated with [Claude Code]" - "Co-Authored-By: Claude noreply@anthropic.com" Any AI tool attribution or signature
- Create clean, professional commit messages without AI references. Use conventional commit format.


## ⚠️ Remember

- **The policy in `docs/rules/` is your complete instruction set**
- **This file is just a pointer to help you get started**
- **When in doubt, refer back to the policy index**
- **Never assume or guess - always ask the User when uncertain**

---

**NOW GO READ:** `docs/rules/project-policy-index.md`

That's where your real instructions are!
