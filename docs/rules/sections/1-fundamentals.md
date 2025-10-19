[← Back to Index](../project-policy-index.md)

---

# Section 1: Fundamentals

**Contains:** Introduction, Core Principles, AI Agent Automation Rules, PRD Alignment, Change Management

---

## 1. Introduction

This policy provides a **single, authoritative, machine-readable source of truth** for AI coding agents and humans. It ensures all work is governed by clear, unambiguous rules and workflows.

### 1.1 Actors

| Actor | Role | Responsibilities |
|-------|------|------------------|
| **User** | Decision Maker | Defines requirements, prioritizes work, approves changes, accountable for all code |
| **AI_Agent** | Executor | Implements User's instructions precisely as defined by PBIs and tasks |

### 1.2 Architectural Compliance

✅ All tasks must be explicitly defined and agreed upon before implementation
✅ All code changes must be associated with a specific task
✅ All PBIs must be aligned with the PRD when applicable

---

## 2. Fundamental Principles

### 2.1 Core Principles

These principles are **MANDATORY** and govern all work:

#### 🎯 Task-Driven Development
- ❌ **NO code changes** without an agreed-upon task
- ✅ Every change must have explicit authorization

#### 📦 PBI Association
- ❌ **NO task creation** without an associated PBI
- ✅ All work traces back to a Product Backlog Item

#### 📖 PRD Alignment
- ✅ If a PRD exists, all PBI features must align with its scope
- ✅ Sense-check all features against PRD requirements

#### 👤 User Authority
- ✅ User is the **sole decider** for scope and design
- ✅ User retains responsibility for all code changes (even if AI implements)

#### 🚫 Prohibition of Unapproved Changes
- ❌ **EXPRESSLY PROHIBITED:** Any changes outside explicit task scope
- ✅ Identify scope creep → roll back → create new task

#### 🔄 Task Status Synchronization
- ✅ Status in `1-tasks.md` **MUST** match status in individual task file
- ✅ Update **BOTH** locations immediately on status change

#### 📄 Controlled File Creation
- ❌ **DO NOT** create files outside defined structures (PBIs, tasks, source code)
- ✅ Get **explicit User confirmation** before creating any standalone files

#### 📚 External Package Research
To avoid hallucinations when using external packages:

1. **Research first:** Use web search to find official documentation
2. **Create guide:** For each package, create `tasks/<task-id>-<package>-guide.md`
3. **Document:** Include:
   - Date stamp
   - Link to original docs
   - API usage examples in project's language
   - Code snippets

**Example:** For task 2-1 using `pg-boss`, create `tasks/2-1-pg-boss-guide.md`

#### ⚙️ Task Granularity
- ✅ Tasks should be **as small as practicable**
- ✅ Must still represent a cohesive, testable unit
- ✅ Break down complex features into multiple smaller tasks

#### 🔁 Don't Repeat Yourself (DRY)
Information should exist in **ONE** location:

- ✅ Task details → in dedicated task files
- ✅ PBI documents → reference tasks, don't duplicate
- ✅ **Exception:** Titles/names can be repeated for clarity

#### 🔢 Use Constants for Values
- ❌ **BAD:** `for (let i = 0; i < 10; i++)`
- ✅ **GOOD:** `const NUM_WEBSITES = 10; for (let i = 0; i < NUM_WEBSITES; i++)`

**Rule:** Any value used more than once → define as named constant

#### 📖 Technical Documentation
For any PBI creating/modifying APIs or interfaces, create documentation including:

- API usage examples and patterns
- Interface contracts and expected behaviors
- Integration guidelines
- Configuration options and defaults
- Error handling and troubleshooting

**Location:** `docs/technical/` or inline code documentation

#### 🔁 Principle of Hierarchical Planning (PBI vs. Task)
To maintain DRY, planning information must be hierarchical:

- PBI Detail Document (prd.md): The Implementation Plan and Testing Strategy sections define the high-level, multi-task strategy (e.g., phases, milestones, testing approach).
- Task Detail Document (<ID>-<TaskNum>.md): The Implementation Plan and Test Plan sections define the detailed, step-by-step, executable actions required to complete one part of the PBI's high-level plan.
- The AI_Agent is responsible for decomposing the high-level PBI plan into detailed, actionable Tasks.

#### ♻️ Legacy Code Prioritization
- ✅ When analyzing a PBI or Task, the **AI_Agent** must *always* prioritize searching for and **leveraging** existing functions, APIs, components, and patterns within the codebase.
- ❌ **PROHIBITED:** Writing new code for functionality that already exists.
- ✅ All implementation plans (`Implementation Plan` / `Implementation Steps`) must explicitly state which legacy assets will be reused.
---

### 2.2 AI Agent Automation Rules

**Purpose:** Defines how AI agents interact with workflow system to ensure synchronization and integrity across all tasks and PBIs.

#### Workflow Interaction Principles

The AI_Agent **MUST** automatically ensure:

1. **Automatic Synchronization**
   - ✅ Status consistency between task files and index files
   - ✅ All file updates occur atomically (same commit)
   - ✅ History logs maintained in both locations
   - ✅ No orphaned tasks or PBIs

2. **Workflow State Management**
   - ✅ Follow defined state transitions (see Section 3.3 for PBI, Section 4.3 for Tasks)
   - ✅ Validate pre-conditions before state changes
   - ✅ Execute all required actions during transitions
   - ✅ Log all state changes with timestamp and user

3. **Integrity Verification**
   - ✅ Before starting any task: verify it exists in both task file AND index
   - ✅ Before status change: validate transition is allowed per workflow rules
   - ✅ After any update: verify both locations are synchronized
   - ✅ Detect and report any inconsistencies immediately

#### Task Management Automation Flow

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
    ├─ Execute version control workflow (Section 4.8)
    └─ Log completion
```

#### PBI Management Automation Flow

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

**2. PBI Plan Generation (User approves idea):**
```
PBI status: Proposed → Agreed (User trigger)
    ↓
AI_Agent MUST:
    ├─ **(NEW) Scan codebase for reusable assets (Discovery)**
    ├─ Create PBI detail document: <PBI-ID>/prd.md (using Template 2)
    ├─ Populate all sections (high-level plan) **using Discovery findings**
    ├─ Link backlog ↔ detail document
    ├─ Update PBI status → PlanInReview
    └─ Log action in history
```

**3. PBIPBI Task Decomposition (User approves plan):**
```
PBI status: PlanInReview → ReadyForTasks (User trigger)
    ↓
AI_Agent MUST:
    ├─ Read approved prd.md
    ├─ Create task list: <PBI-ID>/tasks.md
    ├─ Decompose plan into all task files (<ID>-<TaskNum>.md)
    ├─ Set all new tasks to Proposed
    ├─ Update PBI status → InProgress
    └─ Log action in history
    └─ Begin task execution flow
```

**4. PBI Review (AI finishes all tasks):**
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

#### Automatic Validation Checkpoints

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

#### Error Detection and Recovery

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

#### Automation Safeguards

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

### 2.3 PRD Alignment Check

✅ **MUST** check all PBIs for alignment with PRD
⚠️ **MUST** raise any discrepancies with User

---

### 2.4 Integrity and Sense Checking

✅ All data must be sense-checked for consistency and accuracy

---

### 2.5 Scope Limitations

❌ **NO gold plating** or scope creep
✅ All work scoped to specific task
✅ Improvements → propose as separate tasks

---

### 2.6 Change Management Rules

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

## Navigation

- [← Back to Index](../project-policy-index.md)
- [Next: Section 2 - PBI Management →](./2-pbi-management.md)

---

**End of Section 1**
