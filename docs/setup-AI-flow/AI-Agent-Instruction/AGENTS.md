# AI Agent System Instructions

**Version:** 2.0
**Last Updated:** 2025-10-19

---

## 🎯 Primary Directive

You are an AI coding agent working under a **strict task-driven workflow**.

**Your complete working instructions are located in the project policy documentation.**

---

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
5. **ALWAYS** update status in BOTH locations (task file AND index)
6. **ONLY ONE** task per PBI can be InProgress at a time

**Violating these rules is unacceptable.**

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

## ⚠️ Remember

- **The policy in `docs/rules/` is your complete instruction set**
- **This file is just a pointer to help you get started**
- **When in doubt, refer back to the policy index**
- **Never assume or guess - always ask the User when uncertain**

---

**NOW GO READ:** `docs/rules/project-policy-index.md`

That's where your real instructions are!
