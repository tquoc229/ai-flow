[← Back to Index](../project-policy-index.md) | [← Previous: Section 2](./2-pbi-management.md)

---

# Section 3: Task Management

**Contains:** Task Overview, Document Structure, Workflow States, State Transitions, Synchronization, History Logging, Validation Rules, Version Control

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
---
type: task
priority: medium
created: YYYY-MM-DD
updated: YYYY-MM-DD
estimated_hours: X
---
# Task: [Task-ID] [Task-Name]
[Back to task list](tasks.md)

## Goal
[What this task achieves]

## Context
[Why we're doing this]

## Status History
| Timestamp | Action | From Status | To Status | Details | User |
|-----------|---------|---------|---------|---------|---------|
| ... | ... | ... | ... | ... | ... |

## Requirements
- [ ] Requirement 1
- [ ] Requirement 2

## Implementation Steps
1. Step 1: [Description]
 - ...
2. Step 2: [Description]
 - ...

## Files to Modify/Create
- [ ] path/to/file1.js - [What to do]
- [ ] path/to/new-file.ts - [Create new]

## Testing
**Test Cases**:
- [ ] Test case 1
- [ ] Test case 2

## Success Criteria
- [ ] Implementation complete
- [ ] Tests passing
- [ ] Code follows standards

## References
[Links to relevant docs]

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
| 6-1 | [Define Circuit Breaker](6-1.md) | Proposed | ... |
```

**From task file to task index:**
```markdown
[Back to task list](tasks.md)
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

#### Transition 1: Creating and Auto-Approving a Task

**When:** AI_Agent decomposes PBI plan into tasks (after User approves PBI plan)

**State Change:** (none) → `Proposed` → `Agreed` (automatic)

**Who Can Do This:** AI_Agent (automatic after PBI plan approval)

**Context:**
- This transition happens when PBI status moves to `ReadyForTasks` or `Agreed`
- AI_Agent creates all task files and **automatically sets them to `Agreed`** status
- This eliminates the need for User to approve each task individually
- User has already approved the overall plan at PBI level

**Before Creating Tasks:**
- ✅ PBI plan is approved (PBI status: `ReadyForTasks` or `Agreed`)
- ✅ Tasks decomposed from approved implementation plan
- ✅ Each task represents atomic, implementable unit of work

**AI_Agent MUST Do:**
1. ✅ Create task documentation file named: `<PBI-ID>-<TASK-ID>.md`
2. ✅ Add task entry to task index with link: `[description](<PBI-ID>-<TASK-ID>.md)`
3. ✅ Populate all required sections in task file:
   - Description (clear and specific)
   - Requirements (from PBI breakdown)
   - Implementation Steps (detailed, executable)
   - Test Cases (appropriate to complexity)
   - Files to Modify/Create
4. ✅ Set task status to `Agreed` in **BOTH** task file and index (skip `Proposed` state)
5. ✅ Add entries to task history:
   - First entry - Event_Type: "Created", From: N/A, To: Proposed
   - Second entry - Event_Type: "Auto-Approved", From: Proposed, To: Agreed
   - User: ai-agent
   - Details: "Auto-approved as part of PBI plan approval"

**Rationale for Auto-Approval:**
- User approved the PBI plan which includes task breakdown
- Tasks are derived directly from approved plan
- Reduces overhead - User doesn't need to click "approve" for each task
- User can still modify/reject tasks before they start (Agreed → InProgress requires verification)

**User Can Still:**
- ✅ Review all created tasks
- ✅ Modify task details before work starts
- ✅ Reject specific tasks (mark as N/A or delete)
- ✅ Add new tasks to the list

**Next Step:** Tasks are ready to start - AI_Agent can begin Transition 2 (Starting Work)

---

#### Transition 2: Starting Work on Task

**When:** Ready to begin implementing the task

**State Change:** `Agreed` → `InProgress`

**Who Can Do This:** AI_Agent (after verification checks)

**Before Starting Work:**
- ✅ No other tasks have status `InProgress` for this same PBI
- ✅ Task status is `Agreed` in both task file AND index
- ✅ All dependencies are available
- ✅ Implementation Steps are clear

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
- ✅ All Test Cases from Testing section are implemented and passing
- ✅ Code follows project standards and conventions
- ✅ All modified files are documented in "Files to Modify/Create" section

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
   - Implementation Steps (if approach changed)
   - Testing (if test cases changed)
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
| 6-1 | [Define Circuit Breaker](6-1.md) | Done | Define core state machine |
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

**Parent PBI**: [PBI <PBI-ID>: <PBI Title>](prd.md)

## Task Summary

| Task ID | Name | Status | Description |
| :------ | :--- | :----- | :---------- |
| 6-1 | [Define Circuit Breaker state machine](6-1.md) | Proposed | Define core state machine logic |
| 6-2 | [Implement retry logic with backoff](6-2.md) | Agreed | Add exponential backoff retry mechanism |
| 6-3 | [Add monitoring and metrics](6-3.md) | Proposed | Implement health check and metrics |
```

**Column Definitions:**

| Column | Content | Format |
|--------|---------|--------|
| **Task ID** | Unique identifier | `<PBI-ID>-<TaskNum>` (e.g., `6-1`) |
| **Name** | Task name with link | `[Task Name](<PBI-ID>-<TaskNum>.md)` |
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

## Navigation

- [← Back to Index](../project-policy-index.md)
- [← Previous: Section 2 - PBI Management](./2-pbi-management.md)
- [Next: Section 4 - Testing Strategy →](./4-testing-strategy.md)

---

**End of Section 3**
