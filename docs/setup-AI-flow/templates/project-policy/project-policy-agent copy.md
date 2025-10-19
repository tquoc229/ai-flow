**AI_Agent MUST Do:**
1. ✅ Create task documentation file named: `<PBI-ID>-<TASK-ID>.md`
2. ✅ Add task entry to task index with link: `[description](mdc:<PBI-ID>-<TASK-ID>.md)`
3. ✅ Populate all required sections in task file (Description, Requirements, etc.)
4. ✅ Complete analysis and design work - document in Implementation Plan section
5. ✅ Define test plan appropriate to task complexity
6. ✅ Update task status to `Agreed` in **BOTH** task file and index
7. ✅ Add entries to task history:
   - First entry - Action: "Created", From: N/A, To: Proposed
   - Second entry - Action: "Approved", From: Proposed, To: Agreed
   - User: User name who approved

**Next Step:** Wait for User to approve starting work, or start if authorized

---

#### Transition 2: Starting Work on Task

**When:** Ready to begin implementing the task

**State Change:** `Agreed` → `InProgress`

**Who Can Do This:** AI_Agent (after verification checks)

**Before Starting Work:**
- ✅ No other tasks have status `InProgress` for this same PBI
- ✅ Task status is `Agreed` in both task file AND index
- ✅ All dependencies are available
- ✅ Implementation plan is clear

**AI_Agent MUST Do:**
1. ✅ Check that no other tasks for this PBI are `InProgress`
   - If another task is InProgress → STOP and report to User
2. ✅ Verify task status is `Agreed` in both locations
   - If mismatch found → STOP and report to User
3. ✅ Create feature branch if using version control (e.g., `feature/<task-id>-description`)
4. ✅ Update task status to `InProgress` in **BOTH** task file and index (same commit)
5. ✅ Add entry to task history:
   - Action: "Started Work"
   - From: Agreed, To: InProgress
   - Details: "Began implementation, created feature branch# AI Coding Agent Project Policy

**Version:** 1.0  
**Last Updated:** 2025-10-19  
**Applies To:** All AI coding agents and Vietnamese software engineers

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Fundamental Principles](#fundamental-principles)
3. [Product Backlog Item (PBI) Management](#product-backlog-item-pbi-management)
4. [Task Management](#task-management)
5. [Testing Strategy](#testing-strategy-and-documentation)

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

## 3. Product Backlog Item (PBI) Management

### 3.1 Overview

**Purpose:** Defines how PBIs (Product Backlog Items) are managed throughout their lifecycle.

**Key Concepts:**
- PBIs are the **primary units of work** that define product features and requirements
- Each PBI follows a defined workflow with clear state transitions
- All PBI changes are tracked and logged for full audit trail

---

### 3.2 Backlog Document Structure

**Location:** `docs/delivery/backlog.md`

**Format:**

```markdown
# Product Backlog

| ID | Actor | User Story | Status | Conditions of Satisfaction (CoS) |
|----|-------|------------|--------|----------------------------------|
| 1  | User  | As a user... | Agreed | - Criterion 1<br>- Criterion 2 |
| 2  | Admin | As an admin... | InProgress | - Criterion A<br>- Criterion B |
```

**Core Principles:**
- ✅ Single source of truth for all PBIs
- ✅ Ordered by priority (highest priority at top)
- ✅ All PBIs must have unique IDs
- ✅ Status must match one of the defined values

---

### 3.3 PBI Workflow States

#### State Definitions

| State | Description | What It Means |
|-------|-------------|---------------|
| `Proposed` | Initial state | PBI suggested but not yet approved |
| `Agreed` | Approved | Ready for implementation, tasks can be created |
| `InProgress` | Active work | Implementation underway, tasks being executed |
| `InReview` | Awaiting validation | Implementation complete, pending User review |
| `Done` | Completed | Accepted and merged, no further work needed |
| `Rejected` | Not accepted | Requires rework or has been deprioritized |

---

### 3.4 PBI State Transitions

**Workflow Diagram:**

```
    [Proposed]
        ↓
    [Agreed] ←──────────┐
        ↓               │
    [InProgress]        │
        ↓               │
    [InReview]          │
        ↓               │
    [Done]         [Rejected]
```

---

#### Transition 1: Creating a New PBI

**When:** User or team identifies a new feature or requirement

**State Change:** (none) → `Proposed`

**Who Can Do This:** User or AI_Agent

**Before Creating PBI:**
- ✅ Ensure feature aligns with product vision
- ✅ Check for duplicate PBIs
- ✅ Have clear understanding of the need

**AI_Agent MUST Do:**
1. ✅ Write clear user story using format: "As a [actor], I want [goal], so that [benefit]"
2. ✅ Document Conditions of Satisfaction (CoS) - what "done" means
3. ✅ Assign unique PBI ID (use next sequential number)
4. ✅ Add PBI entry to `backlog.md` with status `Proposed`
5. ✅ Log in PBI history:
   - Action: "Created"
   - Details: Brief description of the PBI
   - User: Who created it

**Next Step:** Wait for User to review and approve

---

#### Transition 2: Approving PBI for Backlog

**When:** User reviews proposed PBI and approves it for implementation

**State Change:** `Proposed` → `Agreed`

**Who Can Do This:** User only

**Before Approving:**
- ✅ PBI aligns with PRD (if PRD exists)
- ✅ User story is clear and complete
- ✅ Conditions of Satisfaction are specific and measurable
- ✅ No duplicate PBIs exist

**AI_Agent MUST Do:**
1. ✅ Create directory: `docs/delivery/<PBI-ID>/`
2. ✅ Create PBI detail document: `docs/delivery/<PBI-ID>/prd.md` with all required sections
3. ✅ Create task list file: `docs/delivery/<PBI-ID>/tasks.md`
4. ✅ Set up bidirectional links:
   - From backlog to detail document
   - From detail document back to backlog
5. ✅ Update PBI status to `Agreed` in backlog
6. ✅ Log in PBI history:
   - Action: "Approved for Backlog"
   - Details: Why approved, any PRD alignment notes
   - User: User name who approved

**Next Step:** Define tasks needed to implement this PBI

---

#### Transition 3: Starting PBI Implementation

**When:** Ready to begin work on PBI, tasks are defined

**State Change:** `Agreed` → `InProgress`

**Who Can Do This:** User or AI_Agent

**Before Starting:**
- ✅ No other PBIs are `InProgress` for the same component
- ✅ Tasks are defined and listed in task list
- ✅ All dependencies are available

**AI_Agent MUST Do:**
1. ✅ Verify no conflicting InProgress PBIs exist
2. ✅ Confirm task list has at least one task
3. ✅ Update PBI status to `InProgress` in backlog
4. ✅ Log in PBI history:
   - Action: "Started Implementation"
   - Details: Number of tasks created, which task starting first
   - User: Who initiated (usually ai-agent)
5. ✅ Begin working on first task (see Section 4.4)

**Next Step:** Work through tasks one by one until all are Done

---

#### Transition 4: Submitting PBI for Review

**When:** All tasks complete, ready for User validation

**State Change:** `InProgress` → `InReview`

**Who Can Do This:** AI_Agent

**Before Submitting:**
- ✅ **EVERY** task for this PBI has status `Done`
- ✅ All Conditions of Satisfaction are met
- ✅ Full test suite passes (including E2E CoS test)
- ✅ All documentation is updated

**AI_Agent MUST Do:**
1. ✅ Verify all tasks show status `Done`
2. ✅ Check each Condition of Satisfaction is satisfied
3. ✅ Run complete test suite and verify all tests pass
4. ✅ Update PBI status to `InReview` in backlog
5. ✅ Notify User that PBI is ready for review
6. ✅ Log in PBI history:
   - Action: "Submitted for Review"
   - Details: Number of tasks completed, test status
   - User: ai-agent

**Next Step:** Wait for User to review and approve or reject

---

#### Transition 5: Approving Completed PBI

**When:** User reviews PBI and confirms it meets all requirements

**State Change:** `InReview` → `Done`

**Who Can Do This:** User only

**Before Approving:**
- ✅ All acceptance criteria verified
- ✅ All tests passing
- ✅ Feature works as expected
- ✅ Quality meets standards

**AI_Agent MUST Do:**
1. ✅ Record completion date
2. ✅ Update PBI status to `Done` in backlog
3. ✅ Archive related task documentation (move to archive or mark as historical)
4. ✅ Log in PBI history:
   - Action: "Approved"
   - Details: Completion notes, what was delivered
   - User: User name who approved
5. ✅ Notify stakeholders of PBI completion

**Next Step:** PBI is complete, move to next priority item

---

#### Transition 6: Rejecting PBI After Review

**When:** User reviews PBI and finds issues that need to be fixed

**State Change:** `InReview` → `Rejected`

**Who Can Do This:** User only

**Before Rejecting:**
- User has identified specific issues or gaps

**AI_Agent MUST Do:**
1. ✅ Document specific reasons for rejection (from User feedback)
2. ✅ List all required changes or rework needed
3. ✅ Update PBI detail document with review feedback
4. ✅ Update PBI status to `Rejected` in backlog
5. ✅ Log in PBI history:
   - Action: "Rejected"
   - Details: Specific reasons for rejection
   - User: User name who rejected
6. ✅ Notify team of required changes

**Next Step:** Address feedback and reopen PBI (see next transition)

---

#### Transition 7: Reopening Rejected PBI

**When:** Issues from rejection have been addressed

**State Change:** `Rejected` → `InProgress`

**Who Can Do This:** User or AI_Agent (after User approval)

**Before Reopening:**
- ✅ All rejection feedback has been addressed
- ✅ User approves resuming work

**AI_Agent MUST Do:**
1. ✅ Create new tasks for required changes (if needed)
2. ✅ Update PBI detail document with changes made
3. ✅ Update PBI status to `InProgress` in backlog
4. ✅ Log in PBI history:
   - Action: "Reopened"
   - Details: What was fixed, what tasks created
   - User: Who authorized reopening
5. ✅ Notify team that work has resumed

**Next Step:** Complete remaining work and resubmit for review

---

#### Transition 8: Deprioritizing PBI

**When:** PBI needs to be postponed or moved down in priority

**State Change:** `Agreed` or `InProgress` → `Proposed`

**Who Can Do This:** User only

**Before Deprioritizing:**
- User has decided this work should be postponed

**AI_Agent MUST Do:**
1. ✅ Document reason for deprioritization (from User)
2. ✅ Pause any in-progress tasks (update their status to document the pause)
3. ✅ Move PBI to lower priority in backlog (reorder if needed)
4. ✅ Update PBI status to `Proposed` in backlog
5. ✅ Log in PBI history:
   - Action: "Deprioritized"
   - Details: Reason for deprioritization
   - User: User name who deprioritized
6. ✅ Notify stakeholders of priority change

**Next Step:** PBI goes back to Proposed state, can be re-approved later

---

### 3.5 PBI History Logging

**Location:** In `docs/delivery/backlog.md` under "## PBI History" section

**Purpose:** Track all changes to PBIs for complete audit trail

**Required Fields:**

| Field | Format | Description |
|-------|--------|-------------|
| **Timestamp** | `YYYY-MM-DD HH:MM:SS` | When the change occurred |
| **PBI_ID** | String | Which PBI was affected |
| **Action** | String | What happened (plain language) |
| **Details** | String | Additional context about the change |
| **User** | String | Who made the change |

**How to Log Actions:**

Use **plain language** that describes what you did:

| When you... | Log this Action |
|-------------|-----------------|
| Create a new PBI | "Created" |
| Approve PBI for backlog | "Approved for Backlog" |
| Start implementing | "Started Implementation" |
| Submit for review | "Submitted for Review" |
| Approve completed PBI | "Approved" |
| Reject PBI | "Rejected" |
| Reopen after rejection | "Reopened" |
| Lower priority | "Deprioritized" |

**Example History:**

```markdown
## PBI History

| Timestamp | PBI_ID | Action | Details | User |
|-----------|--------|--------|---------|------|
| 2025-10-19 14:30:00 | PBI-1 | Created | Initial creation of user authentication PBI | Julian |
| 2025-10-19 15:00:00 | PBI-1 | Approved for Backlog | Approved by Product Owner, aligned with PRD section 3 | Julian |
| 2025-10-20 09:15:00 | PBI-1 | Started Implementation | Created 5 tasks, began work on task 1-1 | ai-agent |
| 2025-10-22 16:45:00 | PBI-1 | Submitted for Review | All 5 tasks completed, tests passing | ai-agent |
| 2025-10-23 10:30:00 | PBI-1 | Approved | All CoS verified and accepted, merged to main | Julian |
```

**More Examples:**

```markdown
## PBI History

| Timestamp | PBI_ID | Action | Details | User |
|-----------|--------|--------|---------|------|
| 2025-10-19 10:00:00 | PBI-2 | Created | Email notification system proposal | Julian |
| 2025-10-19 10:30:00 | PBI-2 | Approved for Backlog | High priority feature for Q4 | Julian |
| 2025-10-20 14:00:00 | PBI-2 | Started Implementation | Created 3 tasks for email service | ai-agent |
| 2025-10-21 09:00:00 | PBI-2 | Blocked | Waiting for email service API credentials | ai-agent |
| 2025-10-22 11:00:00 | PBI-2 | Unblocked | Credentials received, resuming work | Julian |
| 2025-10-24 15:00:00 | PBI-2 | Submitted for Review | Implementation complete, ready for testing | ai-agent |
| 2025-10-25 10:00:00 | PBI-2 | Rejected | Email templates need redesign per UX feedback | Julian |
| 2025-10-25 11:00:00 | PBI-2 | Reopened | Created new task 2-4 for template redesign | ai-agent |
| 2025-10-26 16:00:00 | PBI-2 | Submitted for Review | Templates redesigned and approved | ai-agent |
| 2025-10-27 09:30:00 | PBI-2 | Approved | Feature complete and deployed | Julian |
```

---

### 3.6 PBI Detail Document Structure

**Location:** `docs/delivery/<PBI-ID>/prd.md`

**Purpose:** 
- Serves as mini-PRD for the PBI
- Documents problem space and solution approach
- Provides technical and UX details
- Maintains single source of truth for PBI information

**Required Sections:**

```markdown
# PBI-<ID>: <Title>

## Overview
[High-level summary of what this PBI delivers]

## Problem Statement
[What problem are we solving? Why is it important?]

## User Stories
[Detailed user stories and scenarios]

## Technical Approach
[How we will implement this technically]

## UX/UI Considerations
[User experience and interface design notes]

## Acceptance Criteria
[Specific, measurable criteria for completion - same as CoS in backlog]

## Dependencies
[Other PBIs, external services, or systems this depends on]

## Open Questions
[Unresolved questions that need answers]

## Related Tasks
[Links to task list and individual tasks]
```

**Document Linking:**

**From PBI detail to backlog:**
```markdown
**Backlog Entry:** [View in Backlog](mdc:../backlog.md#user-content-PBI-<ID>)
```

**From backlog to PBI detail:**
```markdown
| 1 | User | As a user... | Agreed | [View Details](mdc:1/prd.md) |
```

**Creation and Ownership:**
- ✅ Created automatically when PBI moves from `Proposed` → `Agreed`
- ✅ Maintained by implementing team member
- ✅ Reviewed during PBI review process
- ✅ Updated throughout PBI lifecycle

---

## 4. Task Management

### 4.1 Overview

**Purpose:** Defines how tasks are created, executed, and tracked as the smallest units of work within PBIs.

**Key Concepts:**
- Tasks are **atomic units of work** that implement specific parts of a PBI
- Each task follows a defined workflow with clear state transitions
- All task changes are synchronized between task file and index
- Only one task per PBI should be InProgress at any time

---

### 4.2 Task Document Structure

**Location:** `docs/delivery/<PBI-ID>/`

**File Naming Convention:**

| File Type | Pattern | Example |
|-----------|---------|---------|
| Task List | `tasks.md` | `docs/delivery/6/tasks.md` |
| Task Details | `<PBI-ID>-<TASK-ID>.md` | `docs/delivery/6/6-1.md` |

**Required Sections in Task Files:**

```markdown
# [Task-ID] [Task-Name]

## Description
[Clear description of what needs to be accomplished]

## Status History
[Table with all status changes - see Section 4.6]

## Requirements
[Specific requirements and acceptance criteria]

## Implementation Plan
[Step-by-step approach to implementation]

## Test Plan
[Testing approach - proportional to complexity]

## Verification
[How to verify the implementation works]

## Files Modified
[List of all files changed by this task]
```

**Core Principles:**

✅ Each task has its own dedicated markdown file  
✅ Files created **IMMEDIATELY** when task added to index  
✅ Naming convention strictly followed  
✅ All required sections must be present and complete  
✅ Links maintained bidirectionally (index ↔ task file)

**Linking Patterns:**

**From task index to task file:**
```markdown
| 6-1 | [Define Circuit Breaker](mdc:6-1.md) | Proposed | ... |
```

**From task file to task index:**
```markdown
[Back to task list](mdc:tasks.md)
```

---

### 4.3 Task Workflow States

#### State Definitions

| State | Description | What It Means |
|-------|-------------|---------------|
| `Proposed` | Initial state | Task defined but not yet approved by User |
| `Agreed` | Approved | User approved, analysis complete, ready for implementation |
| `InProgress` | Active work | AI_Agent actively implementing the task |
| `Review` | Awaiting validation | Implementation complete, pending User review |
| `Done` | Completed | User approved, changes merged, task closed |
| `Blocked` | Cannot proceed | External dependency or issue preventing progress |

---

### 4.4 Task State Transitions

**Workflow Diagram:**

```
    [Proposed]
        ↓
    [Agreed]
        ↓
    [InProgress] ←→ [Blocked]
        ↓
    [Review]
        ↓
    [Done]
```

---

#### Transition 1: Approving a Task

**When:** User reviews proposed task and approves it for implementation

**State Change:** `Proposed` → `Agreed`

**Who Can Do This:** User only

**Before Approving:**
- ✅ Task description is clear and complete
- ✅ Task is properly prioritized in backlog
- ✅ Task aligns with parent PBI goals

**AI_Agent MUST Do:**
1. ✅ Create task documentation file named: `<PBI-ID>-<TASK-ID>.md`
2. ✅ Add task entry to task index with link: `[description](mdc:<PBI-ID>-<TASK-ID>.md)`
3. ✅ Populate all required sections in task file (Description, Requirements, etc.)
4. ✅ Complete analysis and design work - document in Implementation Plan section
5. ✅ Define test plan appropriate to task complexity
6. ✅ Update task status to `Agreed` in **BOTH** task file and index
7. ✅ Add entries to task history:
   - First entry - Event_Type: "Created", From: N/A, To: Proposed
   - Second entry - Event_Type: "Approved", From: Proposed, To: Agreed
   - User: User name who approved

**Next Step:** Wait for User to approve starting work, or start if authorized

---

#### Transition 2: Starting Work on Task

**When:** Ready to begin implementing the task

**State Change:** `Agreed` → `InProgress`

**Who Can Do This:** AI_Agent (after verification checks)

**Before Starting Work:**
- ✅ No other tasks have status `InProgress` for this same PBI
- ✅ Task status is `Agreed` in both task file AND index
- ✅ All dependencies are available
- ✅ Implementation plan is clear

**AI_Agent MUST Do:**
1. ✅ Check that no other tasks for this PBI are `InProgress`
   - If another task is InProgress → STOP and report to User
2. ✅ Verify task status is `Agreed` in both locations
   - If mismatch found → STOP and report to User
3. ✅ Create feature branch if using version control (e.g., `feature/<task-id>-description`)
4. ✅ Update task status to `InProgress` in **BOTH** task file and index (same commit)
5. ✅ Add entry to task history:
   - Event_Type: "Started Work"
   - From: Agreed, To: InProgress
   - Details: "Began implementation, created feature branch" (or similar)
   - User: ai-agent
6. ✅ Update task documentation with start time and assignee
7. ✅ Begin implementation following the Implementation Plan

**Next Step:** Implement the task according to plan

---

#### Transition 3: Submitting Task for Review

**When:** Implementation complete, ready for User to review

**State Change:** `InProgress` → `Review`

**Who Can Do This:** AI_Agent

**Before Submitting:**
- ✅ All requirements from Requirements section are implemented
- ✅ All tests from Test Plan are written and passing
- ✅ Code follows project standards and conventions
- ✅ All modified files are documented in "Files Modified" section

**AI_Agent MUST Do:**
1. ✅ Run all relevant tests and verify they pass
   - If tests fail → fix issues before submitting
2. ✅ Update task documentation:
   - Implementation Plan: mark completed steps
   - Files Modified: list all changed files
   - Add implementation notes if needed
3. ✅ Update task status to `Review` in **BOTH** task file and index (same commit)
4. ✅ Create pull request if applicable with:
   - Title: `[<task-id>] <task-description>`
   - Link to task file in description
5. ✅ Add entry to task history:
   - Event_Type: "Submitted for Review"
   - From: InProgress, To: Review
   - Details: "Implementation complete, all tests passing" (or similar)
   - User: ai-agent
6. ✅ Notify User that task is ready for review

**Next Step:** Wait for User to review and approve or reject

---

#### Transition 4: Approving Completed Task

**When:** User reviews task and confirms it meets all requirements

**State Change:** `Review` → `Done`

**Who Can Do This:** User only

**Before Approving:**
- ✅ All acceptance criteria are met
- ✅ Tests are passing
- ✅ Code quality is acceptable
- ✅ Implementation matches requirements

**AI_Agent MUST Do:**
1. ✅ **Review Next Tasks in Task List:**
   - Look at subsequent tasks in the task list
   - Check if they are still relevant given current implementation
   - Ask User: "Should I modify or remove any subsequent tasks based on what was implemented?"
   - Document User's response
2. ✅ If User approves any task modifications:
   - Update affected task files
   - Update task index
   - Document changes in history
3. ✅ Merge changes to main branch (if using version control and applicable)
4. ✅ Update task status to `Done` in **BOTH** task file and index (same commit)
5. ✅ Add entry to task history: timestamp, "Status Change", Review → Done, User name
6. ✅ Execute version control workflow:
   ```bash
   git acp "<task-id> <task-description>"
   ```
7. ✅ Archive task documentation (if project archives completed tasks)
8. ✅ Notify stakeholders of task completion

**Next Step:** Task complete, ready to start next task in PBI

---

#### Transition 5: Rejecting Task (Needs Rework)

**When:** User reviews task and finds issues that need fixing

**State Change:** `Review` → `InProgress`

**Who Can Do This:** User only

**Before Rejecting:**
- User has identified specific issues or gaps

**AI_Agent MUST Do:**
1. ✅ Document specific rejection reasons (from User feedback)
2. ✅ Update task documentation with review feedback in Description or new "Review Feedback" section
3. ✅ Update task status to `InProgress` in **BOTH** task file and index (same commit)
4. ✅ Add entry to task history: timestamp, "Status Change", Review → InProgress, User name, with rejection reasons
5. ✅ Identify specific changes needed to address feedback
6. ✅ Resume implementation work to fix identified issues

**Next Step:** Fix issues and resubmit for review

---

#### Transition 6: Updating Task Requirements (During Review)

**When:** During review, User identifies that requirements need significant changes

**State Change:** `Review` → `InProgress`

**Who Can Do This:** User only

**Before Making This Transition:**
- Requirements or implementation approach changed significantly

**AI_Agent MUST Do:**
1. ✅ Document the nature of significant changes to:
   - Requirements section (if requirements changed)
   - Implementation Plan (if approach changed)
   - Test Plan (if testing needs changed)
2. ✅ Update task status to `InProgress` in **BOTH** task file and index (same commit)
3. ✅ Add entry to task history: timestamp, "Significant Update", Review → InProgress, reason for update, User name
4. ✅ Notify stakeholders that additional implementation work is needed
5. ✅ Resume development work to address updated requirements

**Next Step:** Implement the updated requirements

---

#### Transition 7: Blocking a Task

**When:** Task cannot proceed due to external dependency or issue

**State Change:** `InProgress` → `Blocked`

**Who Can Do This:** AI_Agent or User

**Before Blocking:**
- Clear blocker has been identified that prevents progress

**AI_Agent MUST Do:**
1. ✅ Stop all work on this task immediately
2. ✅ Document specific blocking reason in task file:
   - What is blocking the task?
   - What dependency is missing?
   - What decision is needed?
   - What external issue exists?
3. ✅ Identify any dependencies or issues causing the block
4. ✅ Update task status to `Blocked` in **BOTH** task file and index (same commit)
5. ✅ Add entry to task history: timestamp, "Status Change", InProgress → Blocked, blocking reason, who identified it
6. ✅ Notify User and relevant stakeholders immediately with:
   - What is blocked
   - Why it's blocked
   - What's needed to unblock
7. ✅ Consider creating new tasks to address blockers (if applicable)

**Next Step:** Wait for blocker to be resolved

---

#### Transition 8: Unblocking a Task

**When:** The blocking issue has been resolved

**State Change:** `Blocked` → `InProgress`

**Who Can Do This:** AI_Agent or User (whoever resolves the blocker)

**Before Unblocking:**
- ✅ Blocking issue has been fully resolved
- ✅ All dependencies are now available
- ✅ Any required decisions have been made

**AI_Agent MUST Do:**
1. ✅ Document how the blocking issue was resolved
2. ✅ Update task file with resolution details in Description or dedicated section
3. ✅ Update task status to `InProgress` in **BOTH** task file and index (same commit)
4. ✅ Add entry to task history: timestamp, "Status Change", Blocked → InProgress, resolution details, who unblocked
5. ✅ Resume work on the task from where it was stopped
6. ✅ Notify User and stakeholders that work has resumed

**Next Step:** Continue implementing the task

---

### 4.5 Task Status Synchronization

**⚠️ CRITICAL RULE:** Task status **MUST** be synchronized between two locations:
1. Task detail file (`<PBI-ID>-<TASK-ID>.md`)
2. Task index file (`tasks.md`)

**Synchronization Requirements:**

✅ **Atomic Updates:**
- Update both locations in the **SAME** commit
- Never update one without the other

✅ **Status History:**
- Always add entry to task history when status changes
- Include timestamp, event type, from/to status, details, user

✅ **Pre-Work Verification:**
- Before starting any work, verify status matches in both locations
- If mismatch found, stop and report to User

✅ **Status Mismatch Resolution:**
- If mismatch detected, immediately update both to most recent status
- Document the correction in task history

**Example - Task File (`6-1.md`):**

```markdown
## Status History

| Timestamp | Action | From | To | Details | User |
|-----------|--------|------|-----|---------|------|
| 2025-10-19 15:02:00 | Created | N/A | Proposed | Task file created for Circuit Breaker | Julian |
| 2025-10-19 16:15:00 | Approved | Proposed | Agreed | User approved task and analysis | Julian |
| 2025-10-20 09:00:00 | Started Work | Agreed | InProgress | Began implementation | ai-agent |
| 2025-10-20 14:30:00 | Submitted for Review | InProgress | Review | Implementation complete | ai-agent |
| 2025-10-21 10:00:00 | Approved | Review | Done | Reviewed and approved | Julian |
```

**Example - Task Index (`tasks.md`):**

```markdown
| Task ID | Name | Status | Description |
| :------ | :--- | :----- | :---------- |
| 6-1 | [Define Circuit Breaker](mdc:6-1.md) | Done | Define core state machine |
```

---

### 4.6 Task Concurrency Limit

**⚠️ MANDATORY RULE:** Only **ONE** task per PBI may be `InProgress` at any given time.

**Rationale:**
- Maintains focus and clarity
- Prevents conflicting changes
- Simplifies status tracking
- Easier to manage and review

**Exception:**
- User may explicitly approve multiple concurrent tasks in special cases
- Must be documented in task history
- Approval required each time

**Enforcement:**

```
Before starting ANY task:
    ↓
Check: Are there other InProgress tasks for this PBI?
    ↓
Yes → STOP and report to User
    ↓
No → Proceed with start_work transition
```

---

### 4.7 Task History Logging

**Location:** Task markdown file under `## Status History` section

**Purpose:** Track all changes to tasks for complete audit trail

**Required Fields:**

| Field | Format | Description |
|-------|--------|-------------|
| **Timestamp** | `YYYY-MM-DD HH:MM:SS` | When the change occurred |
| **Action** | String | What happened (plain language) |
| **From_Status** | String | Previous status (use N/A for creation) |
| **To_Status** | String | New status after change |
| **Details** | String | Additional context about the change |
| **User** | String | Who made the change |

**How to Log Actions:**

Use **plain language** that describes what you did:

| When you... | Log this Action |
|-------------|-----------------|
| Create new task | "Created" |
| Approve task | "Approved" |
| Start working | "Started Work" |
| Submit for review | "Submitted for Review" |
| Approve completed task | "Approved" |
| Reject task | "Rejected" |
| Update requirements | "Requirements Updated" |
| Block task | "Blocked" |
| Unblock task | "Unblocked" |

**Example History:**

```markdown
## Status History

| Timestamp | Action | From Status | To Status | Details | User |
|-----------|--------|-------------|-----------|---------|------|
| 2025-10-19 15:30:00 | Created | N/A | Proposed | Initial task creation for adding logging | Julian |
| 2025-10-19 16:00:00 | Approved | Proposed | Agreed | Task approved by PO, analysis complete | Julian |
| 2025-10-20 09:15:00 | Started Work | Agreed | InProgress | Began implementation, created feature branch | ai-agent |
| 2025-10-20 14:45:00 | Submitted for Review | InProgress | Review | All requirements met, tests passing | ai-agent |
| 2025-10-21 10:30:00 | Approved | Review | Done | Code reviewed and merged to main | Julian |
```

**More Examples:**

**Example 1: Task with Rejection**

```markdown
## Status History

| Timestamp | Action | From Status | To Status | Details | User |
|-----------|--------|-------------|-----------|---------|------|
| 2025-10-19 14:00:00 | Created | N/A | Proposed | Task created for API endpoint | Julian |
| 2025-10-19 14:30:00 | Approved | Proposed | Agreed | Analysis approved, ready to implement | Julian |
| 2025-10-20 09:00:00 | Started Work | Agreed | InProgress | Started implementation | ai-agent |
| 2025-10-20 16:00:00 | Submitted for Review | InProgress | Review | Implementation complete | ai-agent |
| 2025-10-21 10:00:00 | Rejected | Review | InProgress | Missing error handling for edge case | Julian |
| 2025-10-21 14:00:00 | Submitted for Review | InProgress | Review | Added comprehensive error handling | ai-agent |
| 2025-10-22 09:00:00 | Approved | Review | Done | All issues resolved, approved | Julian |
```

**Example 2: Task with Blocking**

```markdown
## Status History

| Timestamp | Action | From Status | To Status | Details | User |
|-----------|--------|-------------|-----------|---------|------|
| 2025-10-19 10:00:00 | Created | N/A | Proposed | Task for third-party API integration | Julian |
| 2025-10-19 11:00:00 | Approved | Proposed | Agreed | Approved for implementation | Julian |
| 2025-10-20 09:00:00 | Started Work | Agreed | InProgress | Began API integration work | ai-agent |
| 2025-10-20 11:00:00 | Blocked | InProgress | Blocked | Waiting for API credentials from vendor | ai-agent |
| 2025-10-22 14:00:00 | Unblocked | Blocked | InProgress | Credentials received, resuming work | Julian |
| 2025-10-22 17:00:00 | Submitted for Review | InProgress | Review | Integration complete and tested | ai-agent |
| 2025-10-23 10:00:00 | Approved | Review | Done | Integration working correctly | Julian |
```

**Example 3: Task with Requirements Update**

```markdown
## Status History

| Timestamp | Action | From Status | To Status | Details | User |
|-----------|--------|-------------|-----------|---------|------|
| 2025-10-19 09:00:00 | Created | N/A | Proposed | Task for user dashboard | Julian |
| 2025-10-19 10:00:00 | Approved | Proposed | Agreed | Initial requirements approved | Julian |
| 2025-10-20 09:00:00 | Started Work | Agreed | InProgress | Started dashboard implementation | ai-agent |
| 2025-10-21 15:00:00 | Submitted for Review | InProgress | Review | Dashboard complete per requirements | ai-agent |
| 2025-10-22 10:00:00 | Requirements Updated | Review | InProgress | Need to add real-time updates feature | Julian |
| 2025-10-23 16:00:00 | Submitted for Review | InProgress | Review | Added real-time updates via WebSocket | ai-agent |
| 2025-10-24 09:00:00 | Approved | Review | Done | All requirements met including real-time | Julian |
```

---

### 4.8 Task Validation Rules

**Purpose:** Ensure all tasks adhere to required standards and workflows.

**Core Validation Rules:**

✅ **Task Association:**
- Every task MUST be associated with an existing PBI
- Task ID MUST be unique within parent PBI

✅ **Workflow Compliance:**
- Tasks MUST follow defined workflow states
- Status transitions MUST be valid per Section 4.4
- All pre-conditions MUST be met before transitions

✅ **Documentation Requirements:**
- All required sections MUST be complete before marking `Done`
- History MUST be maintained for all status changes
- Files modified MUST be documented

✅ **File Consistency:**
- Every task in index MUST have corresponding markdown file
- Task descriptions MUST be linked using correct pattern
- Bidirectional links MUST be maintained

**Pre-Implementation Validation:**

Before starting work on any task, AI_Agent MUST verify:

1. ✅ Task exists in both task file AND index
2. ✅ Task status is correct in both locations
3. ✅ No other tasks are InProgress for same PBI
4. ✅ All task files that will be modified are accessible
5. ✅ Task ID will be referenced in all commits

**Error Prevention Protocol:**

```
Error detected (file inaccessible, status mismatch, etc.)
    ↓
⛔ STOP all work immediately
    ↓
📢 Report to User with details:
   - What is the error?
   - Where is it located?
   - What is the recommended fix?
    ↓
⏸️ Wait for User guidance
    ↓
✅ Apply fix atomically (both locations)
    ↓
✅ Verify resolution
    ↓
✅ Log correction in history
```

**Change Management Validation:**

✅ **Commit Requirements:**
- Reference task ID in ALL commit messages
- Format: `<task-id> <task-description>`

✅ **Scope Verification:**
- All changes MUST be within task scope
- Document any deviations in task history
- Scope creep MUST be identified and addressed

✅ **Status Tracking:**
- Update task status according to workflow
- Link all changes to task
- Maintain audit trail

---

### 4.9 Version Control for Task Completion

**Purpose:** Ensure consistent version control practices when completing tasks.

**Commit Message Format:**

```
<task-id> <task-description>
```

**Examples:**

```bash
1-7 Add pino logging to help debug database connection issues
2-3 Implement exponential backoff for API retries
6-1 Define and implement Circuit Breaker state machine
```

**Pull Request Format:**

**Title:**
```
[<task-id>] <task-description>
```

**Description Template:**
```markdown
## Task Reference
Link to task: [Task <task-id>](path/to/task/file.md)

## Changes Made
- Change 1
- Change 2

## Testing
- Test scenario 1: ✅ Pass
- Test scenario 2: ✅ Pass

## Verification
Steps to verify this implementation works correctly.
```

**Automation Workflow:**

When task is marked `Done`, execute:

```bash
git acp "<task-id> <task-description>"
```

**This command performs:**
1. Stages all changes (`git add .`)
2. Creates commit with specified message (`git commit -m "..."`)
3. Pushes to remote branch (`git push`)

**Post-Commit Verification:**

✅ Verify commit appears in task history  
✅ Confirm task status updated to `Done` in both locations  
✅ Validate commit message follows format  
✅ Check pull request is created (if applicable)

---

### 4.10 Task Index File Structure

**Location:** `docs/delivery/<PBI-ID>/tasks.md`

**Purpose:** Provide consolidated view of all tasks for a PBI

**Required Format:**

```markdown
# Tasks for PBI <PBI-ID>: <PBI Title>

This document lists all tasks associated with PBI <PBI-ID>.

**Parent PBI**: [PBI <PBI-ID>: <PBI Title>](mdc:prd.md)

## Task Summary

| Task ID | Name | Status | Description |
| :------ | :--- | :----- | :---------- |
| 6-1 | [Define Circuit Breaker state machine](mdc:6-1.md) | Proposed | Define core state machine logic |
| 6-2 | [Implement retry logic with backoff](mdc:6-2.md) | Agreed | Add exponential backoff retry mechanism |
| 6-3 | [Add monitoring and metrics](mdc:6-3.md) | Proposed | Implement health check and metrics |
```

**Column Definitions:**

| Column | Content | Format |
|--------|---------|--------|
| **Task ID** | Unique identifier | `<PBI-ID>-<TaskNum>` (e.g., `6-1`) |
| **Name** | Task name with link | `[Task Name](mdc:<PBI-ID>-<TaskNum>.md)` |
| **Status** | Current status | One of: `Proposed`, `Agreed`, `InProgress`, `Review`, `Done`, `Blocked` |
| **Description** | Brief summary | One-sentence description of task objective |

**⚠️ Content Restrictions:**

The Task Summary table **MUST** contain:
- ✅ Only the four columns specified above
- ✅ Only task information (no additional notes or sections mixed in)
- ✅ Proper markdown linking to task files

**PROHIBITED** unless User explicitly approves:
- ❌ Additional columns
- ❌ Mixed content (notes, comments within table)
- ❌ Unlinked task entries

**Maintenance:**

✅ Update immediately when task status changes  
✅ Keep synchronized with individual task files  
✅ Add new tasks at bottom of table (FIFO order)  
✅ Never remove tasks (keep for historical record)

---

## 5. Testing Strategy and Documentation

### 5.1 Overview

**Purpose:** Ensures testing is approached thoughtfully, appropriately scoped, and well-documented.

**Key Concepts:**
- Testing effort should match complexity and risk
- Test plans must be proportional to task complexity
- Avoid over-engineering test plans for simple tasks
- Focus on quality over quantity

---

### 5.2 Testing Principles

#### Risk-Based Testing

✅ **Prioritize testing based on:**
- Complexity of the feature
- Risk of failure and impact
- Criticality to system functionality
- Likelihood of change

#### Test Pyramid Strategy

```
        /\
       /E2E\      ← Few: Critical user paths only
      /------\
     /  INT   \   ← Some: Component interactions
    /----------\
   /    UNIT    \  ← Many: Individual functions
  /--------------\
```

**Distribution Guidelines:**
- **Unit Tests (Base):** Most numerous, fast, isolated
- **Integration Tests (Middle):** Moderate number, verify interactions
- **E2E Tests (Top):** Fewest, slowest, critical paths only

#### Clarity and Maintainability

✅ Tests should be:
- Clear and easy to understand
- Concise without complex logic
- Self-documenting with good names
- Easy to maintain and update

#### Automation First

✅ Automate all tests for:
- Consistent verification
- Repeatable execution
- Fast feedback loops
- Regression prevention

---

### 5.3 Test Scoping by Type

#### Unit Tests

**Focus:** Individual functions, methods, or classes **in isolation**

**Characteristics:**
- ✅ Test one thing at a time
- ✅ Fast execution (milliseconds)
- ✅ No external dependencies
- ✅ Predictable and deterministic

**Mocking Strategy:**
- ✅ Mock **ALL** external dependencies
- ✅ Mock database connections
- ✅ Mock API clients
- ❌ **DO NOT** test package API methods directly (covered by package's own tests)

**Scope:**
- ✅ Verify logic within the unit
- ✅ Test edge cases
- ✅ Test error handling
- ✅ Validate input/output transformations

**Example Scenarios:**
```javascript
// Unit test example
describe('calculateDiscount', () => {
  it('applies 10% discount for orders over $100', () => {
    const result = calculateDiscount(150);
    expect(result).toBe(15);
  });

  it('returns 0 for orders under $100', () => {
    const result = calculateDiscount(50);
    expect(result).toBe(0);
  });

  it('handles zero amount', () => {
    const result = calculateDiscount(0);
    expect(result).toBe(0);
  });
});
```

---

#### Integration Tests

**Focus:** Multiple components working together as a subsystem

**Characteristics:**
- ✅ Test component interactions
- ✅ Verify data flows correctly
- ✅ Test against real infrastructure when possible
- ✅ Moderate execution time (seconds)

**What to Test:**
- API endpoint + service layer + database + job queue
- Service orchestration across multiple services
- Data persistence and retrieval
- Message queue integration

**Mocking Strategy:**

✅ **Mock external third-party services:**
- External APIs (Firecrawl, Gemini, etc.)
- Third-party payment gateways
- External authentication services

✅ **Use REAL instances for internal infrastructure:**
- Database (test database instance)
- Message queues (pg-boss test instance)
- Cache (Redis test instance)

**Rationale:** Testing against real infrastructure validates actual behavior and catches integration issues.

**When to Start with Integration Tests:**
- ✅ Complex features with significant component interaction
- ✅ New workflows or orchestration logic
- ✅ Features spanning multiple services

**Example Scenarios:**
```javascript
// Integration test example
describe('User Registration Flow', () => {
  let testDb;
  let testQueue;

  beforeAll(async () => {
    testDb = await setupTestDatabase();
    testQueue = await setupTestQueue();
  });

  it('should register user and send welcome email', async () => {
    const userData = { email: 'test@example.com', password: 'secure123' };
    
    // Call API endpoint
    const response = await request(app)
      .post('/api/register')
      .send(userData);
    
    // Verify database
    const user = await testDb.users.findOne({ email: userData.email });
    expect(user).toBeDefined();
    
    // Verify job queued
    const jobs = await testQueue.fetch('send-email');
    expect(jobs).toHaveLength(1);
    expect(jobs[0].data.type).toBe('welcome');
  });
});
```

---

#### End-to-End (E2E) Tests

**Focus:** Complete application flow from user perspective

**Characteristics:**
- ✅ Test through UI (typically)
- ✅ Simulate real user interactions
- ✅ Slowest execution (minutes)
- ✅ Most brittle, highest maintenance

**Scope:**
- ✅ Reserved for **critical user paths** only
- ✅ Happy path scenarios
- ✅ Key business workflows
- ❌ **NOT** for every feature or edge case

**Example Scenarios:**
- User registration → login → purchase → checkout
- Admin creates content → publishes → user views
- Critical security flows

**Best Practices:**
- ✅ Keep E2E tests minimal
- ✅ Focus on business value
- ✅ Use page object pattern
- ✅ Make tests resilient to UI changes

---

### 5.4 Test Plan Documentation Strategy

#### PBI-Level Testing

**Conditions of Satisfaction (CoS):**

Located in `docs/delivery/<PBI-ID>/prd.md`, the CoS define:
- ✅ High-level success criteria for the PBI
- ✅ What "done" means for the PBI
- ✅ Acceptance criteria from User perspective

**No Need to Duplicate:** Detailed test plans are **NOT** duplicated at PBI level if covered by tasks.

**E2E CoS Test Task (Required):**

Every PBI task list **MUST** include a dedicated E2E testing task:

**Naming Pattern:** `<PBI-ID>-E2E-CoS-Test.md`

**Purpose:**
- ✅ Holistic end-to-end testing
- ✅ Verify PBI's overall CoS are met
- ✅ Test workflows spanning multiple implementation tasks
- ✅ Validate complete feature from user perspective

**Example:**
```markdown
# 6-E2E-CoS-Test: Verify Circuit Breaker Resilience

## Test Plan

### Objective
Verify the complete circuit breaker implementation meets all CoS for PBI-6.

### Test Scenarios

1. **Circuit Opens on Repeated Failures**
   - Simulate 5 consecutive failures
   - Verify circuit opens
   - Confirm requests are immediately rejected

2. **Circuit Recovers After Cooldown**
   - Wait for cooldown period
   - Verify circuit moves to half-open
   - Confirm successful request closes circuit

3. **Metrics and Monitoring**
   - Verify all state transitions logged
   - Confirm metrics captured correctly
   - Validate alerts triggered appropriately
```

---

#### Task-Level Test Plan Proportionality

**⚠️ CRITICAL PRINCIPLE:** Test plans **MUST** be proportional to task complexity and risk.

**Complexity Assessment:**

| Complexity | Characteristics | Test Plan Detail |
|------------|-----------------|------------------|
| **Simple** | Constants, interfaces, configuration | Minimal - compilation and basic integration |
| **Basic** | Simple functions, basic integrations | Moderate - core functionality and error patterns |
| **Complex** | Multi-service integration, complex logic | Detailed - scenarios, edge cases, mocking strategy |

---

##### Simple Tasks - Minimal Test Plans

**When to Use:**
- Defining constants or configuration
- Creating type definitions or interfaces
- Simple utility functions
- Basic model definitions

**Test Plan Focus:**
- ✅ Compilation success
- ✅ Basic integration works
- ✅ No syntax errors

**Example:**

```markdown
## Test Plan

### Objective
Verify constant definitions compile correctly and are accessible.

### Test Approach
- TypeScript compilation passes without errors
- Constants can be imported by other modules
- Values are correctly typed

### Success Criteria
- ✅ `npm run build` succeeds
- ✅ No TypeScript errors
- ✅ Constants accessible in dependent modules
```

**What to Avoid:**
- ❌ Elaborate test scenarios for simple constants
- ❌ Multiple test files for basic definitions
- ❌ Over-engineering for straightforward tasks

---

##### Basic Tasks - Moderate Test Plans

**When to Use:**
- Basic function implementation
- Simple service methods
- Straightforward integrations
- Standard CRUD operations

**Test Plan Focus:**
- ✅ Core functionality works
- ✅ Basic error handling
- ✅ Follows existing patterns

**Example:**

```markdown
## Test Plan

### Objective
Verify user validation function works correctly.

### Test Scope
- `validateUserInput()` function
- Error handling for invalid inputs

### Test Scenarios

1. **Valid Input**
   - Provide valid user data
   - Expect: Validation passes

2. **Invalid Email**
   - Provide malformed email
   - Expect: Validation error returned

3. **Missing Required Fields**
   - Omit required fields
   - Expect: Specific error messages

### Success Criteria
- All test scenarios pass
- Error messages are clear and helpful
- Function follows existing validation patterns
```

---

##### Complex Tasks - Detailed Test Plans

**When to Use:**
- Multi-service integration
- Complex business logic
- New architectural patterns
- Critical system functionality

**Test Plan Focus:**
- ✅ Comprehensive scenario coverage
- ✅ Detailed mocking strategy
- ✅ Edge cases and error scenarios
- ✅ Performance considerations

**Example:**

```markdown
## Test Plan

### Objective
Verify circuit breaker implementation handles all failure scenarios correctly.

### Test Scope
- Circuit breaker state machine
- Integration with retry mechanism
- Database connection handling
- Job queue integration

### Environment & Setup
- Test database with sample data
- Mock external API (Firecrawl)
- Real pg-boss instance for job queue
- Metrics collection enabled

### Mocking Strategy

**External Services:**
- ✅ Firecrawl API: Full mock with configurable responses
- ✅ Gemini API: Mock to simulate rate limiting

**Internal Infrastructure:**
- ✅ Database: Real PostgreSQL test instance
- ✅ Job Queue: Real pg-boss test instance
- ✅ Cache: Real Redis test instance

### Key Test Scenarios

#### 1. Happy Path - Circuit Remains Closed
**Setup:**
- Circuit starts in closed state
- External service responding normally

**Steps:**
1. Execute 10 consecutive successful requests
2. Verify all requests complete successfully
3. Check circuit state remains closed

**Expected:**
- All requests succeed
- Circuit state: Closed
- Metrics show 0 failures

---

#### 2. Circuit Opens on Repeated Failures
**Setup:**
- Circuit in closed state
- Configure failure threshold: 5 failures

**Steps:**
1. Simulate 5 consecutive failures from external service
2. Verify circuit opens after 5th failure
3. Attempt 6th request
4. Confirm request immediately rejected (circuit open)

**Expected:**
- Circuit state: Open after 5 failures
- 6th request fails immediately without calling service
- Metrics record failure count and state change

---

#### 3. Half-Open State and Recovery
**Setup:**
- Circuit in open state
- Wait for cooldown period (30 seconds)

**Steps:**
1. Wait for cooldown timeout
2. Verify circuit moves to half-open
3. Send successful test request
4. Confirm circuit closes

**Expected:**
- Circuit state transitions: Open → Half-Open → Closed
- Successful request in half-open closes circuit
- Metrics record state transitions

---

#### 4. Failure in Half-Open Reopens Circuit
**Setup:**
- Circuit in half-open state

**Steps:**
1. Send request that fails
2. Verify circuit reopens immediately
3. Confirm cooldown timer resets

**Expected:**
- Circuit state: Open
- New cooldown period started
- Metrics record reopening

---

#### 5. Concurrent Requests During State Transitions
**Setup:**
- Circuit approaching failure threshold

**Steps:**
1. Send 10 concurrent requests
2. Simulate failures on all requests
3. Verify circuit opens correctly
4. Confirm no race conditions

**Expected:**
- Circuit opens after threshold reached
- Subsequent requests handled correctly
- No inconsistent state

---

#### 6. Database Connection Resilience
**Setup:**
- Temporarily disconnect database

**Steps:**
1. Trigger operation requiring database
2. Verify circuit breaker activates
3. Reconnect database
4. Verify recovery after cooldown

**Expected:**
- Database errors trigger circuit breaker
- Circuit recovers when database available
- No data loss or corruption

### Edge Cases

1. **System restart during open state:**
   - Circuit state persists across restarts
   - Cooldown timer resumes correctly

2. **Very high request volume:**
   - Circuit breaker handles 1000+ req/sec
   - State transitions remain accurate

3. **Partial failures (some succeed, some fail):**
   - Failure rate calculated correctly
   - Circuit opens only when threshold exceeded

### Performance Requirements

- Circuit state check: < 1ms overhead
- State transition: < 5ms
- Metrics recording: Non-blocking

### Success Criteria

✅ All happy path scenarios pass  
✅ All error scenarios handled correctly  
✅ All edge cases covered  
✅ Code coverage > 85% for circuit breaker module  
✅ Integration tests pass with real infrastructure  
✅ No memory leaks detected  
✅ Performance requirements met  
✅ Metrics accurately recorded
```

---

### 5.5 Test Plan Requirements

**Location:** Every task file in `## Test Plan` section

**Requirement:** Every task involving code implementation **MUST** include a test plan.

**Proportionality Matrix:**

| Task Type | Test Plan Detail | Key Elements |
|-----------|------------------|--------------|
| **Simple** | Minimal | Objective, basic success criteria |
| **Basic** | Moderate | Objective, test scope, 3-5 scenarios, success criteria |
| **Complex** | Comprehensive | All sections including mocking strategy, edge cases, performance |

**Test Plan Sections by Complexity:**

**Simple Tasks:**
```markdown
## Test Plan
- Objective: What are we verifying?
- Success Criteria: How do we know it works?
```

**Basic Tasks:**
```markdown
## Test Plan
- Objective
- Test Scope
- Test Scenarios (3-5)
- Success Criteria
```

**Complex Tasks:**
```markdown
## Test Plan
- Objective
- Test Scope
- Environment & Setup
- Mocking Strategy
- Key Test Scenarios (5-10)
- Edge Cases
- Performance Requirements (if applicable)
- Success Criteria
```

**⚠️ Completion Prerequisite:** 

A task **CANNOT** be marked as `Done` unless:
- ✅ Test plan is complete and appropriate to complexity
- ✅ All tests in plan are implemented
- ✅ All tests are **PASSING**

**Review Guidelines:**
- Test plans reviewed for appropriateness to complexity
- Avoid over-engineering for simple tasks
- Ensure adequate coverage for complex tasks
- Update plan if requirements change significantly

---

### 5.6 Test Distribution Strategy

**Avoid Duplication:**

❌ **DO NOT** repeat detailed edge case testing across individual implementation tasks

✅ **DO** concentrate complex scenarios in dedicated E2E testing tasks

**Task-Level Testing Focus:**

Individual implementation tasks should verify:
- ✅ Specific functionality works as intended
- ✅ Integration with broader system
- ✅ Basic error handling
- ✅ Follows existing patterns

**E2E Testing Focus:**

Dedicated E2E CoS test tasks should verify:
- ✅ Complex integration testing
- ✅ Error scenarios across system
- ✅ Full workflow validation
- ✅ User acceptance criteria
- ✅ Performance and scalability

**Example Distribution:**

```
PBI-6: Circuit Breaker Implementation
├─ Task 6-1: Define state machine
│  └─ Test Plan: State transitions work correctly (unit tests)
├─ Task 6-2: Implement retry logic
│  └─ Test Plan: Retry mechanism works (unit + basic integration)
├─ Task 6-3: Add monitoring
│  └─ Test Plan: Metrics captured correctly (unit tests)
└─ Task 6-E2E-CoS-Test: Full circuit breaker testing
   └─ Test Plan: Complete scenarios, edge cases, integration tests
```

---

### 5.7 Test Implementation Guidelines

**Test File Organization:**

```
project/
├── src/
│   ├── services/
│   │   └── user.service.ts
│   └── utils/
│       └── validator.ts
└── test/
    ├── unit/              # Mirror source structure
    │   ├── services/
    │   │   └── user.service.test.ts
    │   └── utils/
    │       └── validator.test.ts
    └── integration/       # By feature or module
        ├── api/
        │   └── user-registration.test.ts
        └── workflows/
            └── circuit-breaker.test.ts
```

**Naming Conventions:**

✅ **Unit Tests:**
- Pattern: `<source-file>.test.ts`
- Example: `user.service.test.ts`

✅ **Integration Tests:**
- Pattern: `<feature-name>.test.ts`
- Example: `user-registration-flow.test.ts`

**Test File Structure:**

```typescript
describe('ComponentName', () => {
  // Setup
  beforeAll(() => {
    // One-time setup
  });

  beforeEach(() => {
    // Per-test setup
  });

  afterEach(() => {
    // Per-test cleanup
  });

  afterAll(() => {
    // One-time cleanup
  });

  // Test suites
  describe('methodName', () => {
    it('should do something when condition', () => {
      // Arrange
      const input = ...;
      
      // Act
      const result = methodName(input);
      
      // Assert
      expect(result).toBe(...);
    });
  });
});
```

**Best Practices:**

✅ **Clear test names:**
```typescript
// ❌ Bad
it('test1', () => { ... });

// ✅ Good  
it('should return user when valid ID provided', () => { ... });
```

✅ **Arrange-Act-Assert pattern:**
```typescript
it('should calculate discount correctly', () => {
  // Arrange
  const orderAmount = 150;
  const expectedDiscount = 15;
  
  // Act
  const discount = calculateDiscount(orderAmount);
  
  // Assert
  expect(discount).toBe(expectedDiscount);
});
```

✅ **One assertion per test (when possible):**
```typescript
// Each test verifies one specific behavior
it('should return discount for orders over $100', () => {
  expect(calculateDiscount(150)).toBe(15);
});

it('should return zero for orders under $100', () => {
  expect(calculateDiscount(50)).toBe(0);
});
```

✅ **Descriptive test suites:**
```typescript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user with valid data', () => { ... });
    it('should throw error for duplicate email', () => { ... });
    it('should hash password before saving', () => { ... });
  });
});
```

---

## 📚 Quick Reference Guide

### Common Workflow Patterns

#### Pattern 1: Starting a New PBI

**Step-by-Step:**

```
1. Create PBI entry in backlog.md
   ├─ Assign unique ID (sequential)
   ├─ Write user story: "As a [actor], I want [goal], so that [benefit]"
   ├─ Define Conditions of Satisfaction
   └─ Set status to Proposed

2. User reviews and approves
   └─ Status: Proposed → Agreed

3. AI_Agent creates PBI structure
   ├─ Create: docs/delivery/<PBI-ID>/prd.md
   ├─ Create: docs/delivery/<PBI-ID>/tasks.md
   ├─ Populate all required sections
   ├─ Set up bidirectional linking
   └─ Log approval in PBI history

4. Define tasks in tasks.md
   ├─ Break down PBI into atomic tasks
   ├─ Create task file for each: <PBI-ID>-<TASK-ID>.md
   ├─ Link tasks in task index
   └─ All tasks start as Proposed

5. Ready to start implementation
   └─ PBI status: Agreed → InProgress
```

---

#### Pattern 2: Working on a Task

**Complete Workflow:**

```
Step 1: Pre-Work Validation
├─ Check task exists in both task file AND index
├─ Verify status is "Agreed" in both locations
├─ Confirm no other tasks are "InProgress" for same PBI
└─ Review implementation plan is clear

Step 2: Start Work (Agreed → InProgress)
├─ AI_Agent updates status to "InProgress" in BOTH locations
├─ Log start event in task history with timestamp
├─ Create feature branch (if using version control)
└─ Begin implementing according to plan

Step 3: During Implementation
├─ Follow the implementation plan step by step
├─ Only make changes within task scope
├─ Run tests continuously as you code
├─ Document all files you modify
└─ Include task ID in every commit message

Step 4: Submit for Review (InProgress → Review)
├─ Verify all requirements are implemented
├─ Run complete test suite - all must pass
├─ Update task file with implementation details
├─ List all modified files in task documentation
├─ Update status to "Review" in BOTH locations
├─ Notify User that task is ready
└─ Log submission in task history

Step 5: User Reviews Task
├─ If User Approves (Review → Done):
│  ├─ Review next tasks - are they still relevant?
│  ├─ Confirm with User about next tasks
│  ├─ Update status to "Done" in BOTH locations
│  ├─ Run: git acp "<task-id> <description>"
│  └─ Log completion with timestamp
│
└─ If User Rejects (Review → InProgress):
   ├─ Document specific rejection reasons
   ├─ Update task with User's feedback
   ├─ Update status to "InProgress" in BOTH locations
   └─ Resume work to fix issues
```

**Key Points to Remember:**
- ✅ ALWAYS update both task file AND index together
- ✅ NEVER have two tasks InProgress for same PBI
- ✅ ALWAYS log status changes with timestamp
- ✅ ALWAYS reference task ID in commits

---

#### Pattern 3: Handling Blocked Tasks

**Complete Workflow:**

```
Step 1: Task Gets Blocked
├─ Identify what is preventing progress:
│  ├─ External dependency not available?
│  ├─ Waiting for User decision?
│  ├─ Technical issue can't be resolved?
│  └─ Missing critical information?
└─ Stop all work immediately

Step 2: Mark Task as Blocked (InProgress → Blocked)
├─ Document the specific blocking reason clearly:
│  ├─ What exactly is blocking?
│  ├─ What dependency is missing?
│  ├─ What decision is needed from User?
│  └─ What information is required?
├─ Update status to "Blocked" in BOTH locations
├─ Log blocking event in task history
└─ Notify User immediately with blocking details

Step 3: While Task is Blocked
├─ Monitor the blocker for resolution
├─ Keep User updated on any changes
├─ Create new tasks to address blocker (if you can)
└─ Do NOT work on this task until unblocked

Step 4: Blocker Gets Resolved
├─ Verify the blocker is fully resolved
├─ Dependency is now available OR
├─ User has made the decision OR
└─ Technical issue is fixed

Step 5: Unblock Task (Blocked → InProgress)
├─ Document how blocker was resolved
├─ Update task file with resolution details
├─ Update status to "InProgress" in BOTH locations
├─ Log unblocking event in task history
├─ Resume work from where you stopped
└─ Notify User and stakeholders work resumed
```

**Common Blocking Reasons:**
- 🔴 Waiting for external API credentials
- 🔴 User needs to make architectural decision
- 🔴 Dependency on another team's work
- 🔴 Required third-party service unavailable
- 🔴 Missing design specifications
- 🔴 Technical limitation discovered

**What to Do While Blocked:**
- ✅ Document everything about the blocker
- ✅ Propose solutions if possible
- ✅ Work on other non-blocked tasks (different PBI)
- ❌ Do NOT try to work around the blocker
- ❌ Do NOT start other tasks in same PBI

---

#### Pattern 4: Creating Task Documentation

**Task File Template:**

```markdown
# <Task-ID> <Task-Name>

[Back to task list](mdc:tasks.md)

## Description

[Clear description of what needs to be accomplished]
[Include context, motivation, and expected outcome]

## Status History

| Timestamp | Event Type | From Status | To Status | Details | User |
|-----------|------------|-------------|-----------|---------|------|
| YYYY-MM-DD HH:MM:SS | Created | N/A | Proposed | Initial creation | username |
| YYYY-MM-DD HH:MM:SS | Approved | Proposed | Agreed | Task approved and analysis complete | username |

## Requirements

- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

## Implementation Plan

### Step 1: [First step]
[Details...]

### Step 2: [Second step]
[Details...]

### Step 3: [Final step]
[Details...]

## Test Plan

[Proportional to task complexity - see Section 5.4]

### Objective
[What are we verifying?]

### Test Scenarios
1. Scenario 1
2. Scenario 2

### Success Criteria
- ✅ Criterion 1
- ✅ Criterion 2

## Verification

[How to verify the implementation works correctly]

**Steps:**
1. Step 1
2. Step 2
3. Expected result

## Files Modified

- `path/to/file1.ts`
- `path/to/file2.ts`
- `path/to/file3.test.ts`
```

---

#### Pattern 5: Status Synchronization Check

**Before ANY Status Change:**

```javascript
// Pseudo-code for validation
function validateStatusChange(taskId, newStatus) {
  // 1. Read status from task file
  const fileStatus = readTaskFile(taskId).status;
  
  // 2. Read status from index
  const indexStatus = readTaskIndex(taskId).status;
  
  // 3. Verify they match
  if (fileStatus !== indexStatus) {
    throw new Error(
      `Status mismatch for ${taskId}: 
       File: ${fileStatus}, Index: ${indexStatus}`
    );
  }
  
  // 4. Verify transition is valid
  if (!isValidTransition(fileStatus, newStatus)) {
    throw new Error(
      `Invalid transition: ${fileStatus} → ${newStatus}`
    );
  }
  
  // 5. Update both atomically
  updateTaskFile(taskId, newStatus);
  updateTaskIndex(taskId, newStatus);
  logHistoryEntry(taskId, fileStatus, newStatus);
}
```

---

#### Pattern 6: External Package Integration

**When Using External Package:**

```
1. Research First
   ├─ Search official documentation
   ├─ Understand API usage
   ├─ Check compatibility
   └─ Review examples

2. Create Package Guide
   ├─ File: tasks/<task-id>-<package-name>-guide.md
   ├─ Date stamp the document
   ├─ Link to original docs
   ├─ Include API usage examples
   ├─ Add code snippets in project language
   └─ Document gotchas and best practices

3. Reference in Task
   ├─ Link package guide from task file
   ├─ Use guide as implementation reference
   └─ Update guide if API changes

Example:
Task 2-1 using pg-boss
├─ Create: tasks/2-1-pg-boss-guide.md
├─ Document pg-boss API usage
└─ Reference in task 2-1.md
```

---

### Common Commands and Patterns

#### Git Workflow for Task Completion

```bash
# When task moves to Done status

# Option 1: Using automation helper
git acp "1-7 Add pino logging to help debug database connection issues"

# Option 2: Manual steps
git add .
git commit -m "1-7 Add pino logging to help debug database connection issues"
git push origin feature/1-7-add-pino-logging
```

#### Creating New PBI Structure

```bash
# After PBI approved (Proposed → Agreed)

# Create directory
mkdir -p docs/delivery/<PBI-ID>

# Create files
touch docs/delivery/<PBI-ID>/prd.md
touch docs/delivery/<PBI-ID>/tasks.md

# Populate templates
# - prd.md: Use PBI detail template (Section 3.6)
# - tasks.md: Use task index template (Section 4.10)
```

#### Checking Task Status

```bash
# Check for status mismatches

# 1. Read task file status
grep "Status" docs/delivery/<PBI-ID>/<PBI-ID>-<TASK-ID>.md

# 2. Read index status
grep "<TASK-ID>" docs/delivery/<PBI-ID>/tasks.md

# 3. Compare - they must match!
```

---

### Decision Trees

#### Decision: Should I Search for Package Documentation?

```
Are you about to use an external package?
    ↓
  YES → Do you fully understand its API?
    ↓              ↓
   NO             YES
    ↓              ↓
Search web     Verify your
for official   understanding
docs first     is current
    ↓              ↓
Create package  Proceed with
guide document  implementation
```

---

#### Decision: What Test Plan Detail Level?

```
Assess task complexity
    ↓
Simple? (constants, interfaces, config)
    ↓
  YES → Minimal test plan
    |   - Objective
    |   - Success criteria
    ↓
   NO
    ↓
Basic? (simple functions, CRUD)
    ↓
  YES → Moderate test plan
    |   - Objective
    |   - Test scope
    |   - 3-5 scenarios
    |   - Success criteria
    ↓
   NO
    ↓
Complex? (multi-service, complex logic)
    ↓
  YES → Detailed test plan
        - Objective
        - Test scope
        - Environment & setup
        - Mocking strategy
        - 5-10 scenarios
        - Edge cases
        - Success criteria
```

---

#### Decision: Can I Start This Task?

**Step-by-Step Decision Process:**

```
Question 1: Is the task status "Agreed"?
    ↓
   NO → ❌ STOP. You cannot start this task.
    |   → The task needs User approval first.
    |   → Wait for User to review and approve.
    ↓
  YES → Continue to next check
    ↓

Question 2: Does the status match in BOTH locations?
    ↓
    Check task file status: _______
    Check index file status: _______
    ↓
   NO → ❌ STOP. Status mismatch detected!
    |   → Report this issue to User immediately.
    |   → Provide details: which file has which status.
    |   → Wait for User to fix synchronization.
    ↓
  YES → Continue to next check
    ↓

Question 3: Are there any other tasks "InProgress" for this PBI?
    ↓
  YES → ❌ STOP. Cannot start this task.
    |   → Only ONE task per PBI can be InProgress.
    |   → Report which other task is InProgress.
    |   → Wait for that task to complete first.
    ↓
   NO → Continue to next check
    ↓

Question 4: Are all dependencies available?
    ↓
   NO → ❌ STOP. Task is blocked.
    |   → Mark task as Blocked.
    |   → Document what dependencies are missing.
    |   → Notify User about the blocker.
    ↓
  YES → ✅ All checks passed!
    ↓
    
    ✅ You can start this task!
    ↓
    Proceed with "Starting Work on Task" transition:
    1. Update status to "InProgress" in BOTH locations
    2. Log start event in task history
    3. Begin implementation
```

**Quick Checklist:**
- [ ] Task status is "Agreed" ✅
- [ ] Status matches in task file and index ✅
- [ ] No other tasks "InProgress" for this PBI ✅
- [ ] All dependencies available ✅

**If ALL checked → Start work!**

---

### Validation Checklists

#### Before Starting Any Task

- [ ] Task exists in task file (`<PBI-ID>-<TASK-ID>.md`)
- [ ] Task exists in task index (`tasks.md`)
- [ ] Status is `Agreed` in task file
- [ ] Status is `Agreed` in task index
- [ ] No other tasks `InProgress` for this PBI
- [ ] All dependencies are available
- [ ] Implementation plan is clear
- [ ] Test plan is defined

---

#### Before Submitting Task for Review

- [ ] All requirements implemented
- [ ] All tests written and passing
- [ ] Test plan requirements met
- [ ] Code follows project standards
- [ ] Files modified documented in task file
- [ ] Task documentation updated with implementation details
- [ ] No scope creep (all changes within task scope)
- [ ] Commits reference task ID

---

#### Before Marking Task as Done

- [ ] User has reviewed and approved
- [ ] All acceptance criteria verified
- [ ] All tests passing
- [ ] Status updated in BOTH locations
- [ ] Next tasks reviewed for relevance
- [ ] User confirmed about next tasks
- [ ] Version control workflow executed
- [ ] Completion logged in history

---

#### Before Marking PBI as Done

- [ ] ALL tasks have status `Done`
- [ ] All Conditions of Satisfaction met
- [ ] Full test suite passing
- [ ] Documentation updated
- [ ] E2E CoS test completed and passing
- [ ] User approval received
- [ ] Completion date recorded
- [ ] History logged

---

## ⚠️ Common Pitfalls to Avoid

### File Management Pitfalls

❌ **Creating files without User confirmation**
- Always get explicit approval for new standalone files
- Only create files within defined structures (PBI, task, source code)

❌ **Orphaned task files**
- Every task in index MUST have corresponding markdown file
- Create task file IMMEDIATELY when adding to index

❌ **Broken links**
- Always use `mdc:` protocol for internal links
- Test links after creation
- Maintain bidirectional linking

---

### Status Management Pitfalls

❌ **Updating only one location**
- ALWAYS update both task file AND index atomically
- Use same commit for both updates

❌ **Skipping status history**
- ALWAYS log status changes with timestamp and user
- Include meaningful details about the change

❌ **Invalid transitions**
- Follow defined workflow states
- Verify transition is allowed before executing

---

### Workflow Pitfalls

❌ **Multiple InProgress tasks**
- Only ONE task per PBI should be InProgress
- Get User approval for exceptions

❌ **Scope creep**
- Stay within defined task scope
- Create new tasks for additional work
- Roll back unauthorized changes

❌ **Missing User approval**
- Never start work on `Proposed` tasks
- Wait for explicit User approval

---

### Testing Pitfalls

❌ **Over-engineering simple test plans**
- Match test plan detail to task complexity
- Simple tasks need simple test plans

❌ **Skipping E2E CoS test task**
- Every PBI MUST have dedicated E2E testing task
- Don't mark PBI done without E2E tests

❌ **Testing package APIs directly**
- Don't test external package methods in unit tests
- Packages have their own tests

---

### Documentation Pitfalls

❌ **Duplicating information**
- Follow DRY principle
- Reference instead of duplicating
- Exception: Titles/names for clarity

❌ **Using magic numbers**
- Define constants for repeated values
- Use constants for special values
- Name constants descriptively

❌ **Missing package guides**
- Always create guide when using new package
- Research official docs first
- Date-stamp and link to sources

---

## ✅ Best Practices Summary

### Process Best Practices

1. **Always verify before acting**
   - Check task status in both locations
   - Validate transitions are allowed
   - Confirm no conflicts

2. **Keep tasks small and focused**
   - Break down complex features
   - One cohesive unit of work per task
   - Easier to review and test

3. **Document as you go**
   - Update files modified in real-time
   - Log history immediately
   - Keep documentation current

4. **Synchronize atomically**
   - Update both locations in same commit
   - Never update one without the other
   - Verify synchronization after updates

---

### Code Best Practices

1. **Use constants for values**
   - Define once, reference everywhere
   - Name constants descriptively
   - Apply to all repeated values

2. **Follow DRY principle**
   - Single source of truth
   - Reference instead of duplicate
   - Reduce inconsistency risk

3. **Reference task ID in commits**
   - Format: `<task-id> <description>`
   - Every commit traceable to task
   - Clear audit trail

4. **Stay within scope**
   - Only changes authorized by task
   - Identify scope creep early
   - Create new tasks for extras

---

### Testing Best Practices

1. **Match test plan to complexity**
   - Simple tasks → minimal plans
   - Complex tasks → detailed plans
   - Don't over-engineer

2. **Use real infrastructure for integration**
   - Test database instances
   - Real message queues
   - Mock only external services

3. **Focus E2E on critical paths**
   - Reserve for important workflows
   - Don't test everything E2E
   - Keep E2E tests minimal

4. **Automate all tests**
   - Consistent verification
   - Fast feedback
   - Regression prevention

---

### Communication Best Practices

1. **Ask when uncertain**
   - Never assume User intent
   - Clarify ambiguous requirements
   - Confirm before significant changes

2. **Report issues immediately**
   - Don't hide problems
   - Stop work when blocked
   - Provide clear error descriptions

3. **Review next tasks**
   - Before marking current task Done
   - Confirm relevance with User
   - Adapt plan as needed

4. **Keep stakeholders informed**
   - Notify on status changes
   - Report blockers
   - Share completion updates

---

**End of Document**