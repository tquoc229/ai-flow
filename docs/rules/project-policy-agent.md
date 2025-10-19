# AI Coding Agent Project Policy

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

PBIs are the **primary units of work** that define product features and requirements.

---

### 3.2 Backlog Document Rules

**Location:** `docs/delivery/backlog.md`

**Structure:**

```markdown
# Product Backlog

| ID | Actor | User Story | Status | Conditions of Satisfaction (CoS) |
|----|-------|------------|--------|----------------------------------|
| 1  | User  | As a user... | Agreed | - Criterion 1<br>- Criterion 2 |
```

**Principles:**
- ✅ Single source of truth for all PBIs
- ✅ Ordered by priority (highest at top)

---

### 3.3 PBI Workflow

#### Status Definitions

| Status | Meaning |
|--------|---------|
| `Proposed` | Suggested but not yet approved |
| `Agreed` | Approved and ready for implementation |
| `InProgress` | Being actively worked on |
| `InReview` | Implementation complete, awaiting review |
| `Done` | Completed and accepted |
| `Rejected` | Rejected, requires rework or deprioritization |

#### Event Transitions

##### ➡️ `create_pbi` → `Proposed`

**Actions:**
- Define clear user story and acceptance criteria
- Assign unique ID and clear title
- Log creation in PBI history

---

##### ➡️ `propose_for_backlog`: `Proposed` → `Agreed`

**Pre-conditions:**
- ✅ Verify PBI aligns with PRD and project goals
- ✅ Ensure all required information is complete

**Actions:**
- Log approval in PBI history
- Notify stakeholders

---

##### ➡️ `start_implementation`: `Agreed` → `InProgress`

**Pre-conditions:**
- ✅ No other PBIs are InProgress for same component

**Actions:**
- Create tasks for implementing the PBI
- Assign initial tasks
- Log start in PBI history

---

##### ➡️ `submit_for_review`: `InProgress` → `InReview`

**Pre-conditions:**
- ✅ All tasks for PBI are complete
- ✅ All acceptance criteria met

**Actions:**
- Update documentation
- Notify reviewers
- Log submission in PBI history

---

##### ➡️ `approve`: `InReview` → `Done`

**Pre-conditions:**
- ✅ All acceptance criteria met
- ✅ All tests pass

**Actions:**
- Update completion date
- Archive related tasks and documentation
- Log approval in PBI history
- Notify stakeholders

---

##### ➡️ `reject`: `InReview` → `Rejected`

**Actions:**
- Document rejection reasons
- Identify required changes
- Update PBI with review feedback
- Log rejection in PBI history
- Notify team

---

##### ➡️ `reopen`: `Rejected` → `InProgress`

**Actions:**
- Address all feedback
- Update PBI with changes made
- Log reopening in PBI history
- Notify team

---

##### ➡️ `deprioritize`: `(Agreed, InProgress)` → `Proposed`

**Actions:**
- Document deprioritization reason
- Pause in-progress work
- Update status and priority
- Log in PBI history
- Notify stakeholders

---

**⚠️ Important:** All status transitions **MUST** be logged with timestamp and user.

---

### 3.4 PBI History Log

**Location:** `backlog.md` file

**Format:**

| Timestamp | PBI_ID | Event_Type | Details | User |
|-----------|--------|------------|---------|------|
| 2025-10-19 14:30:00 | PBI-1 | create_pbi | Initial creation | Julian |
| 2025-10-19 15:00:00 | PBI-1 | propose_for_backlog | Approved by PO | Julian |

---

### 3.5 PBI Detail Documents

**Location:** `docs/delivery/<PBI-ID>/prd.md`

**Purpose:** Mini-PRD for each PBI

**Required Sections:**

```markdown
# PBI-<ID>: <Title>

## Overview

## Problem Statement

## User Stories

## Technical Approach

## UX/UI Considerations

## Acceptance Criteria

## Dependencies

## Open Questions

## Related Tasks
```

**Linking:**

- ✅ Detail document → Backlog: `[View in Backlog](mdc:../backlog.md#user-content-<PBI-ID>)`
- ✅ Backlog → Detail document: `[View Details](mdc:<PBI-ID>/prd.md)`

**Ownership:**
- Created when PBI moves `Proposed` → `Agreed`
- Maintained by implementing team member
- Reviewed during PBI review process

---

## 4. Task Management

### 4.1 Task Documentation

**Location:** `docs/delivery/<PBI-ID>/`

**File Naming:**
- Task list: `tasks.md`
- Task details: `<PBI-ID>-<TASK-ID>.md` (e.g., `1-1.md`)

**Required Sections:**

```markdown
# [Task-ID] [Task-Name]

## Description

## Status History

## Requirements

## Implementation Plan

## Test Plan

## Verification

## Files Modified
```

---

### 4.2 Principles

✅ Each task has its own dedicated markdown file  
✅ Follow specified naming convention  
✅ All required sections must be present  
✅ When adding task to index, create markdown file **IMMEDIATELY**  
✅ Link using pattern: `[description](mdc:<PBI-ID>-<TASK-ID>.md)`  
✅ Individual task files link back: `[Back to task list](mdc:../<tasks-index-file>.md)`

---

### 4.3 Task Workflow

#### Status Definitions

| Status | Meaning |
|--------|---------|
| `Proposed` | Initial state of newly defined task |
| `Agreed` | User approved, ready for work |
| `InProgress` | AI Agent actively working |
| `Review` | Work complete, awaiting User validation |
| `Done` | User reviewed and approved |
| `Blocked` | Cannot proceed due to dependency/issue |

#### Event Transitions

##### ➡️ `user_approves`: `Proposed` → `Agreed`

**Pre-conditions:**
- ✅ Task description is clear and complete
- ✅ Task properly prioritized

**Actions:**
1. Create task documentation file from `_template.md`
2. Name file `<PBI-ID>-<TASK-ID>.md`
3. Link in tasks index
4. Complete analysis and design in required sections
5. Log status change in task history

---

##### ➡️ `start_work`: `Agreed` → `InProgress`

**Pre-conditions:**
- ✅ No other tasks InProgress for same PBI

**Actions:**
- Create new branch (if using version control)
- Log start time and assignee
- Update task documentation

---

##### ➡️ `submit_for_review`: `InProgress` → `Review`

**Pre-conditions:**
- ✅ All task requirements met
- ✅ All tests pass

**Actions:**
- Update task documentation with implementation details
- Create pull request or mark ready for review
- Notify User
- Log submission in task history

---

##### ➡️ `approve`: `Review` → `Done`

**Pre-conditions:**
- ✅ All acceptance criteria met

**Actions:**
1. Merge changes to main branch (if applicable)
2. Update task documentation
3. Update task status and log completion time
4. Archive task documentation
5. Notify stakeholders
6. Log approval in task history
7. **Review Next Tasks:** Before marking Done, review next task(s) in light of current implementation. Confirm with User whether subsequent tasks:
   - Remain relevant
   - Need modification
   - Have become redundant

---

##### ➡️ `reject`: `Review` → `InProgress`

**Actions:**
- Document rejection reason
- Update task with review feedback
- Notify AI Agent of required changes
- Log rejection

---

##### ➡️ `significant_update`: `Review` → `InProgress`

**Actions:**
- Document nature of changes
- Update task status
- Log update reason
- Notify stakeholders
- Resume development work

---

##### ➡️ `mark_blocked`: `InProgress` → `Blocked`

**Actions:**
- Document blocking reason
- Identify dependencies/issues
- Update task documentation
- Notify stakeholders

---

##### ➡️ `unblock`: `Blocked` → `InProgress`

**Actions:**
- Document resolution
- Update task documentation
- Resume work
- Notify stakeholders

---

### 4.4 Task Status Synchronization

**⚠️ CRITICAL:** Maintain consistency across codebase

✅ **Immediate Updates:** Update both task file AND `1-tasks.md` in same commit  
✅ **Status History:** Always add entry when changing status  
✅ **Status Verification:** Verify status in both locations before starting  
✅ **Status Mismatch:** If found, immediately update both to most recent status

**Example - Task File:**

```markdown
| Timestamp | Event Type | From | To | Details | User |
|-----------|------------|------|-----|---------|------|
| 2025-05-19 15:02:00 | Created | N/A | Proposed | Task file created | Julian |
| 2025-05-19 16:15:00 | Status Update | Proposed | InProgress | Started work | Julian |
```

**Example - 1-tasks.md:**

```markdown
| 1-7 | [Add pino logging...](mdc:1-7.md) | InProgress | Pino logs connection... |
```

---

### 4.5 One In Progress Task Limit

⚠️ **RULE:** Only **ONE** task per PBI should be `InProgress` at any time

**Exception:** User may approve additional concurrent tasks in special cases

---

### 4.6 Task History Log

**Location:** Task's markdown file under "Status History" section

**Required Fields:**

| Field | Format | Description |
|-------|--------|-------------|
| Timestamp | `YYYY-MM-DD HH:MM:SS` | Date and time of change |
| Event_Type | String | Type of event |
| From_Status | String | Previous status |
| To_Status | String | New status |
| Details | String | Description of change |
| User | String | User who initiated change |

**Example:**

```markdown
| Timestamp | Event Type | From Status | To Status | Details | User |
|-----------|------------|-------------|-----------|---------|------|
| 2025-05-16 15:30:00 | Status Change | Proposed | Agreed | Task approved by PO | johndoe |
| 2025-05-16 16:45:00 | Status Change | Agreed | InProgress | Started implementation | ai-agent-1 |
```

---

### 4.7 Task Validation Rules

**Core Rules:**

✅ All tasks associated with existing PBI  
✅ Task IDs unique within parent PBI  
✅ Follow defined workflow states and transitions  
✅ Complete all documentation before marking `Done`  
✅ Maintain history for all status changes  
✅ Only one task per PBI `InProgress` (unless approved)  
✅ Every task in index MUST have corresponding markdown file  
✅ Task descriptions MUST be linked to markdown files

**Pre-Implementation Checks:**

1. ✅ Verify task exists and is in correct status
2. ✅ Document task ID in all related changes
3. ✅ List all files to be modified
4. ✅ Get explicit approval before proceeding

**Error Prevention:**

- ⚠️ If unable to access files → stop and report
- ⚠️ For protected files → provide changes for manual application
- ✅ Verify task status in both locations before starting
- ✅ Document all status checks in history

**Change Management:**

- ✅ Reference task ID in all commit messages
- ✅ Update task status according to workflow
- ✅ Link all changes to task
- ✅ Document any deviations from plan

---

### 4.8 Version Control for Task Completion

**Commit Message Format:**

```
<task_id> <task_description>
```

**Example:**

```
1-7 Add pino logging to help debug database connection issues
```

**Pull Request:**

- **Title:** `[<task_id>] <task_description>`
- **Description:** Include link to task
- **Pre-merge:** Ensure all requirements met

**Automation:**

When task marked `Done`, run:

```bash
git acp "<task_id> <task_description>"
```

This command:
1. Stages all changes
2. Creates commit with specified message
3. Pushes to remote branch

**Verification:**

✅ Commit appears in task history  
✅ Task status updated in both locations  
✅ Commit message follows format

---

### 4.9 Task Index File

**Location:** `docs/delivery/<PBI-ID>/tasks.md`

**Example:** `docs/delivery/6/tasks.md`

**Purpose:** List all tasks for a specific PBI

**Required Structure:**

```markdown
# Tasks for PBI <PBI-ID>: <PBI Title>

This document lists all tasks associated with PBI <PBI-ID>.

**Parent PBI**: [PBI <PBI-ID>: <PBI Title>](mdc:prd.md)

## Task Summary

| Task ID | Name | Status | Description |
| :------ | :--- | :----- | :---------- |
| 6-1 | [Define Circuit Breaker state machine](mdc:6-1.md) | Proposed | Define core state machine |
| 6-2 | [Implement retry logic](mdc:6-2.md) | Agreed | Add exponential backoff |
```

**⚠️ PROHIBITED:** Task Summary table must contain **ONLY** what is specified above, unless User specifically approves additions.

---

## 5. Testing Strategy and Documentation

### 5.1 General Principles

#### Risk-Based Approach
✅ Prioritize testing based on complexity and risk

#### Test Pyramid Adherence
```
     /\
    /E2E\     ← End-to-End (critical paths)
   /------\
  /  INT   \  ← Integration (component interactions)
 /----------\
/    UNIT    \ ← Unit Tests (individual functions)
--------------
```

#### Clarity and Maintainability
✅ Tests should be clear, concise, easy to understand  
✅ Avoid overly complex test logic

#### Automation
✅ Automate tests for consistent verification

---

### 5.2 Test Scoping Guidelines

#### Unit Tests

**Focus:** Individual functions/methods/classes in isolation

**Mocking:**
- ✅ Mock ALL external dependencies
- ❌ **DO NOT** test package API methods directly (covered by package tests)

**Scope:**
- ✅ Verify logic within unit
- ✅ Include edge cases and error handling

---

#### Integration Tests

**Focus:** Verify multiple components working together

**Example:** API endpoint + service layer + database + job queue

**Mocking Strategy:**
- ✅ Mock external third-party services (e.g., Firecrawl, Gemini)
- ✅ For internal infrastructure (database, pg-boss): prefer **real instances** in test environment
- ✅ Avoid deep mocking of internal client libraries

**When to Use:**
- ✅ Start with integration tests for complex features
- ✅ Ensure overall flow and orchestration work
- ✅ Validate against actual behavior

---

#### End-to-End (E2E) Tests

**Focus:** Entire application flow from user perspective

**Scope:**
- ✅ Reserved for critical user paths
- ✅ Test through UI typically

---

### 5.3 Test Plan Documentation Strategy

#### PBI-Level Testing

**Conditions of Satisfaction (CoS):**
- Defined in `docs/delivery/<PBI-ID>/prd.md`
- Inherently define high-level success criteria
- No need to duplicate detailed test plans at PBI level

**E2E CoS Test Task:**
- ✅ **REQUIRED:** Task list must include dedicated "E2E CoS Test" task
- ✅ Example: `<PBI-ID>-E2E-CoS-Test.md`
- ✅ Contains detailed E2E test plan
- ✅ Verifies PBI's overall CoS are met
- ✅ May span multiple implementation tasks

---

#### Task-Level Test Plan Proportionality

**⚠️ Pragmatic Principle:** Test plans **MUST** be proportional to complexity and risk

##### Simple Tasks (constants, interfaces, config)

**Test Plan Focus:**
- ✅ Compilation and basic integration
- ✅ "TypeScript compilation passes without errors"

**Example:**

```markdown
## Test Plan

**Objective:** Verify constant definitions compile correctly

**Success Criteria:**
- TypeScript compilation passes
- Constants accessible from importing modules
```

---

##### Basic Implementation Tasks

**Test Plan Focus:**
- ✅ Core functionality verification
- ✅ Error handling patterns

**Example:**

```markdown
## Test Plan

**Objective:** Verify function can be registered and follows error patterns

**Test Scenarios:**
1. Function registers successfully with system
2. Basic workflow executes without errors
3. Errors follow existing error handling patterns

**Success Criteria:**
- All scenarios pass
- No regressions in existing tests
```

---

##### Complex Implementation Tasks

**Test Plan Focus:**
- ✅ Specific scenarios and edge cases
- ✅ Detailed but not excessive

**Example:**

```markdown
## Test Plan

**Objective:** Verify multi-service integration handles all scenarios

**Test Scope:**
- Service A → Service B communication
- Database transaction handling
- Job queue integration

**Environment & Setup:**
- Test database with sample data
- Mock external API calls
- Local job queue instance

**Mocking Strategy:**
- External API: Full mock
- Database: Real test instance
- Job queue: Real test instance

**Key Test Scenarios:**

1. **Happy Path:**
   - Data flows through all services
   - Transaction commits successfully
   - Job queued and processed

2. **Error Scenarios:**
   - Service B unavailable → retry logic triggered
   - Database constraint violation → transaction rolled back
   - Job queue full → graceful degradation

3. **Edge Cases:**
   - Concurrent requests
   - Large payload handling
   - Timeout scenarios

**Success Criteria:**
- All scenarios pass
- Code coverage > 80% for new code
- No memory leaks detected
```

---

### 5.4 Test Plan Documentation Requirements

**Requirement:** Every task involving code implementation **MUST** include test plan

**Location:** `## Test Plan` section in task detail document

**Content Guidelines:**

| Task Complexity | Required Content |
|-----------------|------------------|
| **Simple** | Success criteria for basic functionality and compilation |
| **Medium** | Above + key test scenarios |
| **Complex** | Full plan: Objectives, Scope, Environment, Mocking, Scenarios, Success Criteria |

**Review:** Test plans reviewed for appropriateness to complexity

**Living Document:** Update if task requirements change significantly

**⚠️ Completion Prerequisite:** Task cannot be `Done` unless tests in plan are **PASSING**

---

### 5.5 Test Distribution Strategy

**Avoid Duplication:**
- ❌ Don't repeat detailed edge cases across individual tasks
- ✅ Concentrate complex scenarios in dedicated E2E testing tasks

**Focus Individual Tasks:**
- ✅ Verify specific functionality works as intended
- ✅ Integration with broader system

**Comprehensive Testing:**
- ✅ Complex integration testing → dedicated testing tasks
- ✅ Error scenarios → E2E CoS tests
- ✅ Full workflow validation → E2E CoS tests

---

### 5.6 Test Implementation Guidelines

**Test File Locations:**

```
project/
├── test/
│   ├── unit/          # Mirror source directory structure
│   │   ├── services/
│   │   └── utils/
│   └── integration/   # Or test/<module>/ (e.g., test/server/)
│       ├── api/
│       └── workflows/
```

**Test Naming:**
- ✅ Clear and descriptive
- ✅ Reflect what is being tested

**Examples:**
- `test/unit/services/user-service.test.ts`
- `test/integration/api/webhook-handler.test.ts`

---

## 📚 Quick Reference

### Common Workflows

#### Starting a New PBI

1. Create PBI in `backlog.md` with status `Proposed`
2. User approves → status changes to `Agreed`
3. Create PBI detail document: `docs/delivery/<PBI-ID>/prd.md`
4. Create task list: `docs/delivery/<PBI-ID>/tasks.md`
5. Create individual tasks as `<PBI-ID>-<TASK-ID>.md`

#### Working on a Task

1. Verify task is in `Agreed` status
2. Use MCP tool: `start_task`
3. Status changes to `InProgress`
4. Implement changes
5. Run tests
6. Use MCP tool: `submit_task_for_review`
7. Status changes to `Review`
8. User approves → status changes to `Done`

#### Creating Task Documentation

```markdown
# 1-7 Add pino logging

## Description
[Clear description of what needs to be done]

## Status History
[Table with all status changes]

## Requirements
- Requirement 1
- Requirement 2

## Implementation Plan
[Step-by-step plan]

## Test Plan
[Appropriate to task complexity]

## Verification
[How to verify it works]

## Files Modified
- `src/services/logger.ts`
- `package.json`
```

---

## ⚠️ Common Pitfalls to Avoid

1. ❌ Creating files without explicit User confirmation
2. ❌ Making changes without associated task
3. ❌ Forgetting to update both task file AND index
4. ❌ Over-engineering test plans for simple tasks
5. ❌ Not documenting external package usage
6. ❌ Using magic numbers instead of constants
7. ❌ Duplicating information across documents
8. ❌ Having multiple tasks `InProgress` simultaneously

---

## ✅ Best Practices

1. ✅ Always verify task status before starting work
2. ✅ Keep tasks small and focused
3. ✅ Document external package APIs before use
4. ✅ Use constants for repeated values
5. ✅ Make test plans proportional to complexity
6. ✅ Update both task file and index together
7. ✅ Link documents properly using `mdc:` protocol
8. ✅ Review next tasks before marking current as Done

---

**End of Document**