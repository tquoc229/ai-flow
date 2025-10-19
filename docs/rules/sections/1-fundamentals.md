# Section 1: Fundamental Principles

[← Back to Index](mdc:../project-policy-index.md)

---

## Overview

This section defines the core principles and rules that govern ALL work in this project. These principles are **MANDATORY** and must be followed at all times.

---

## 1. Core Principles

### 🎯 Task-Driven Development
- ❌ **NO code changes** without an agreed-upon task
- ✅ Every change must have explicit authorization

### 📦 PBI Association
- ❌ **NO task creation** without an associated PBI
- ✅ All work traces back to a Product Backlog Item

### 📖 PRD Alignment
- ✅ If a PRD exists, all PBI features must align with its scope
- ✅ Sense-check all features against PRD requirements

### 👤 User Authority
- ✅ User is the **sole decider** for scope and design
- ✅ User retains responsibility for all code changes (even if AI implements)

### 🚫 Prohibition of Unapproved Changes
- ❌ **EXPRESSLY PROHIBITED:** Any changes outside explicit task scope
- ✅ Identify scope creep → roll back → create new task

### 🔄 Task Status Synchronization
- ✅ Status in `tasks.md` **MUST** match status in individual task file
- ✅ Update **BOTH** locations immediately on status change

### 📄 Controlled File Creation
- ❌ **DO NOT** create files outside defined structures (PBIs, tasks, source code)
- ✅ Get **explicit User confirmation** before creating any standalone files

### 📚 External Package Research
To avoid hallucinations when using external packages:

1. **Research first:** Use web search to find official documentation
2. **Create guide:** For each package, create `tasks/<task-id>-<package>-guide.md`
3. **Document:** Include:
   - Date stamp
   - Link to original docs
   - API usage examples in project's language
   - Code snippets

**Example:** For task 2-1 using `pg-boss`, create `tasks/2-1-pg-boss-guide.md`

### ⚙️ Task Granularity
- ✅ Tasks should be **as small as practicable**
- ✅ Must still represent a cohesive, testable unit
- ✅ Break down complex features into multiple smaller tasks

### 🔁 Don't Repeat Yourself (DRY)
Information should exist in **ONE** location:

- ✅ Task details → in dedicated task files
- ✅ PBI documents → reference tasks, don't duplicate
- ✅ **Exception:** Titles/names can be repeated for clarity

### 🔢 Use Constants for Values
- ❌ **BAD:** `for (let i = 0; i < 10; i++)`
- ✅ **GOOD:** `const NUM_WEBSITES = 10; for (let i = 0; i < NUM_WEBSITES; i++)`

**Rule:** Any value used more than once → define as named constant

### 📖 Technical Documentation
For any PBI creating/modifying APIs or interfaces, create documentation including:

- API usage examples and patterns
- Interface contracts and expected behaviors
- Integration guidelines
- Configuration options and defaults
- Error handling and troubleshooting

**Location:** `docs/technical/` or inline code documentation

---

## 2. AI Agent Automation Rules

**Purpose:** Defines how AI agents interact with workflow system to ensure synchronization and integrity across all tasks and PBIs.

### Workflow Interaction Principles

The AI_Agent **MUST** automatically ensure:

1. **Automatic Synchronization**
   - ✅ Status consistency between task files and index files
   - ✅ All file updates occur atomically (same commit)
   - ✅ History logs maintained in both locations
   - ✅ No orphaned tasks or PBIs

2. **Workflow State Management**
   - ✅ Follow defined state transitions (see Section 2 for PBI, Section 3 for Tasks)
   - ✅ Validate pre-conditions before state changes
   - ✅ Execute all required actions during transitions
   - ✅ Log all state changes with timestamp and user

3. **Integrity Verification**
   - ✅ Before starting any task: verify it exists in both task file AND index
   - ✅ Before status change: validate transition is allowed per workflow rules
   - ✅ After any update: verify both locations are synchronized
   - ✅ Detect and report any inconsistencies immediately

### Task Management Automation Flow

When working with tasks, AI_Agent **AUTOMATICALLY**:

**1. Task Initiation:**
```
User approves task (Proposed → Agreed)
    ↓
AI_Agent MUST:
    ├─ Create task file: <PBI-ID>-<TASK-ID>.md
    ├─ Link in tasks index
    ├─ Populate all required sections
    ├─ Update status in BOTH locations
    └─ Log creation in history
```

**2. Task Execution:**
```
Task status: Agreed → InProgress
    ↓
AI_Agent MUST verify:
    ├─ No other InProgress tasks for same PBI
    ├─ Task status matches in both locations
    └─ All pre-conditions met
    ↓
AI_Agent MUST update:
    ├─ Task file status
    ├─ Index file status
    ├─ Status history in task file
    └─ Timestamp and user info
```

**3. Task Completion:**
```
Task status: InProgress → Review
    ↓
AI_Agent MUST verify:
    ├─ All requirements implemented
    ├─ All tests passing
    └─ Files modified documented
    ↓
AI_Agent MUST update:
    ├─ Task file with implementation details
    ├─ Task file status → Review
    ├─ Index file status → Review
    └─ Status history logged
    ↓
User approves (Review → Done)
    ↓
AI_Agent MUST:
    ├─ Review next task relevance
    ├─ Confirm with User if next tasks still valid
    ├─ Update both locations to Done
    ├─ Execute version control workflow
    └─ Log completion
```

### PBI Management Automation Flow

When working with PBIs, AI_Agent **AUTOMATICALLY**:

**1. PBI Creation:**
```
User creates PBI (→ Proposed)
    ↓
AI_Agent MUST verify:
    ├─ Unique PBI ID
    ├─ Required fields complete
    └─ PRD alignment (if PRD exists)
    ↓
AI_Agent MUST log:
    └─ Creation in PBI history
```

**2. PBI Approval:**
```
PBI status: Proposed → Agreed
    ↓
AI_Agent MUST:
    ├─ Create PBI detail document: <PBI-ID>/prd.md
    ├─ Create task list: <PBI-ID>/tasks.md
    ├─ Populate required sections
    ├─ Link backlog ↔ detail document
    └─ Log approval in history
```

**3. PBI Implementation:**
```
PBI status: Agreed → InProgress
    ↓
AI_Agent MUST verify:
    ├─ No other InProgress PBIs for same component
    └─ Tasks are defined
    ↓
AI_Agent MUST:
    ├─ Update PBI status in backlog
    ├─ Log start in history
    └─ Begin task execution flow
```

**4. PBI Review:**
```
PBI status: InProgress → InReview
    ↓
AI_Agent MUST verify:
    ├─ ALL tasks are Done
    ├─ All CoS met
    └─ Documentation updated
    ↓
AI_Agent MUST:
    ├─ Update PBI status
    ├─ Notify for review
    └─ Log submission
```

### Automatic Validation Checkpoints

AI_Agent performs these checks **AUTOMATICALLY** at each interaction:

**Before ANY code change:**
```
1. ✅ Identify linked PBI and Task
2. ✅ Verify task is in correct status (InProgress)
3. ✅ Confirm change is within task scope
4. ✅ Check task status matches in both locations
```

**Before ANY status change:**
```
1. ✅ Validate transition is allowed per workflow
2. ✅ Verify all pre-conditions met
3. ✅ Prepare to update both locations atomically
```

**After ANY file update:**
```
1. ✅ Verify synchronization between locations
2. ✅ Confirm history logged
3. ✅ Validate all links still work
```

### Error Detection and Recovery

AI_Agent **MUST** detect and report:

❌ **Synchronization Errors:**
- Task status mismatch between file and index
- Orphaned task files (in folder but not in index)
- Missing task files (in index but file doesn't exist)

❌ **Workflow Violations:**
- Multiple InProgress tasks for same PBI
- Invalid state transitions
- Missing required documentation

❌ **Integrity Issues:**
- Broken links between documents
- Missing required sections
- Incomplete history logs

**Recovery Actions:**
```
Error detected
    ↓
1. ⛔ STOP all work immediately
2. 📢 Report error to User with details:
   ├─ What is inconsistent
   ├─ Where the mismatch exists
   └─ Recommended fix
3. ⏸️ Wait for User guidance
4. ✅ Apply fix to both locations atomically
5. ✅ Verify resolution
```

### Automation Safeguards

To ensure proper workflow compliance, AI_Agent:

✅ **NEVER:**
- Makes status changes without updating both locations
- Creates tasks without corresponding files
- Modifies files outside current task scope
- Proceeds with work if validation fails

✅ **ALWAYS:**
- Validates before acting
- Updates atomically
- Logs all changes
- Reports inconsistencies immediately
- Asks User when uncertain

---

## 3. PRD Alignment Check

✅ **MUST** check all PBIs for alignment with PRD
⚠️ **MUST** raise any discrepancies with User

---

## 4. Integrity and Sense Checking

✅ All data must be sense-checked for consistency and accuracy

---

## 5. Scope Limitations

❌ **NO gold plating** or scope creep
✅ All work scoped to specific task
✅ Improvements → propose as separate tasks

---

## 6. Change Management Rules

**Before ANY code change:**

1. ✅ Identify the linked PBI or Task
2. ✅ Verify change is within task scope
3. ✅ If User requests change without task reference:
   - ❌ **DO NOT** proceed with work
   - ✅ Discuss whether to associate with existing task or create new PBI + task

**Change Workflow:**

```
User Request → Identify Task → Verify Scope → Execute OR Create New Task
```

---

## Quick Reference

**Key Questions to Ask:**

1. "Is there an agreed task for this change?"
   - If NO → Cannot proceed, need to create task first

2. "Does this change fall within the task scope?"
   - If NO → This is scope creep, create separate task

3. "Have I updated status in BOTH locations?"
   - If NO → Must update task file AND index together

4. "Are there other InProgress tasks for this PBI?"
   - If YES → Cannot start new task, wait for completion

5. "Do I need to create a new file?"
   - If YES → Get User confirmation first

---

[← Back to Index](mdc:../project-policy-index.md) | [Next: PBI Management →](mdc:2-pbi-management.md)
