# Policy Section 3: Task Management

<critical>This document defines how tasks are created, executed, and tracked as atomic units of work.</critical>
<critical>This is Section 3 of the AI Coding Agent Project Policy.</critical>

---

## Overview

**Tasks** are the smallest atomic units of work within a PBI.

- Each task is independently implementable
- Tasks follow a defined workflow with state transitions
- Only ONE task per PBI can be InProgress at a time (concurrency limit)
- Task status is managed via Single Source of Truth pattern

---

## Task Workflow States

The task lifecycle is defined by states in `ai-flow-config.yaml` → `task.states`.

| State | Description |
|-------|-------------|
| **Proposed** | Task defined but not yet approved by user |
| **Agreed** | Approved by user, ready for implementation |
| **InProgress** | AI agent actively implementing the task |
| **InReview** | Implementation complete, pending user review |
| **Testing** | In testing phase (if applicable) |
| **Blocked** | Cannot proceed due to external blocker |
| **Done** | User approved, changes merged, task closed |
| **Cancelled** | Task cancelled, will not be worked on |

### State Diagram

```
    [Proposed]
        ↓
    [Agreed]
        ↓
    [InProgress] ←→ [Blocked]
        ↓
    [InReview]
        ↓
    [Testing] (optional)
        ↓
    [Done]

    [Cancelled] (from any state except Done)
```

---

## Valid State Transitions

Defined in `ai-flow-config.yaml` → `task.transitions`:

| From | To | Trigger |
|------|-----|---------|
| Proposed | Agreed | User approves task |
| Proposed | Cancelled | User rejects task |
| Agreed | InProgress | AI starts work |
| Agreed | Cancelled | Task no longer needed |
| InProgress | InReview | Implementation complete |
| InProgress | Blocked | External blocker prevents progress |
| InReview | Testing | Ready for testing phase |
| InReview | InProgress | Needs rework after review |
| Testing | Done | Tests passed, user approves |
| Testing | InProgress | Tests failed, needs fixes |
| Blocked | InProgress | Blocker resolved |
| Blocked | Cancelled | Cannot resolve blocker |

**Invalid transitions will be rejected.**

---

## Critical Rules

### 1. Concurrency Limit (MANDATORY)

**Maximum {task.max_concurrent} task(s) InProgress per PBI at any time.**

Default: `1` (configured in `ai-flow-config.yaml`)

**Enforcement:**
- Before starting a task, AI MUST check for other InProgress tasks
- If limit reached, HALT and report to user
- User must complete or cancel active task(s) before starting new one

**Why:** Ensures focus, prevents conflicts, maintains quality

### 2. Status Source of Truth

**Task detail file** (`{pbi_id}/{task_id}.md`) is the ONLY authoritative source for status.

**Task index** (`{pbi_id}/tasks.md`) is AUTO-GENERATED from detail files.

**Process:**
1. AI updates status in task detail file ONLY
2. Pre-commit hook regenerates task index from all detail files
3. Both committed in same commit → always synchronized

**Benefits:**
- Eliminates synchronization errors
- Reduces cognitive load
- Single place to update

### 3. Auto-Approval

Tasks created from an approved PBI plan are **automatically set to "Agreed"**.

**Rationale:** User already approved the high-level plan in PRD, so decomposed tasks are implicitly approved.

### 4. History Logging

ALL state transitions MUST be logged in task file's Status History table.

**Format:**
```markdown
| {ISO8601_timestamp} | {event_type} | {from_state} | {to_state} | {details} | {user} |
```

**Example:**
```markdown
| 2025-10-21T10:30:00Z | State Transition | Agreed | InProgress | Started implementation | ai-agent |
```

---

## Task Document Structure

### Task Index (`tasks.md`)

**Location:** `{pbi_id}/tasks.md`

**Format:** Markdown table (auto-generated)

| Column | Description |
|--------|-------------|
| Task ID | Format: {pbi_id}-{n} |
| Name | Linked to detail file |
| Status | Current state |
| Description | Brief summary |

**Restrictions:**
- ONLY these 4 columns
- NO extra notes or unlinked entries
- AUTO-GENERATED - do not edit manually

### Task Detail File (`{pbi_id}/{task_id}.md`)

**Location:** `{pbi_id}/{pbi_id}-{n}.md`

**Required Sections:**

1. **Goal** - Clear objective statement
2. **Context** - Background from parent PBI
3. **Status History** - Table with ALL state transitions
4. **Requirements** - Specific, measurable requirements
5. **Implementation Plan** - Detailed steps to implement
6. **Files Modified** - List of files to modify/create
7. **Verification** - How to verify completion
8. **Testing** - Test requirements (unit, integration)

**Optional Sections:**
- Implementation Notes (appended after completion)
- Test Results
- Dependencies
- Notes

---

## Task Obsolescence Criteria

After completing any task, AI MUST evaluate remaining tasks for obsolescence.

**A task is OBSOLETE if it meets ANY of these criteria:**

### Criterion 1: Already Satisfied
Requirements fully satisfied by a previous task.

### Criterion 2: Superseded
Implementation diverged from original plan, task no longer needed.

### Criterion 3: External Dependency
Third-party library/service now provides the functionality.

### Criterion 4: Requirements Changed
User explicitly changed PBI requirements, removing need for task.

### Evaluation Process

**When:** After completing any task, when PBI requirements change

**Steps:**
1. List remaining tasks (not Done, not InProgress)
2. Check each against 4 criteria
3. Document findings for flagged tasks
4. Present to user for confirmation
5. Remove/modify per user decision, log action

**Anti-Patterns (NOT obsolete):**
- Task is hard
- Task will take time
- Could be done later

These are NOT valid reasons for obsolescence.

---

## Validation Rules

### Task Creation
1. Must be associated with a PBI
2. Must have all required sections
3. Must follow naming convention: `{pbi_id}-{n}.md`
4. Auto-approved status: "Agreed"

### Task Execution
1. Only 1 task InProgress per PBI
2. Must follow workflow state transitions
3. All changes logged in Status History
4. Files Modified section must be accurate

### Task Completion
1. All requirements implemented
2. All tests passing (if applicable)
3. All verification steps executed
4. Implementation notes documented

---

## Version Control Guidelines

### Commit Message Format

From `ai-flow-config.yaml` → `automation.commit_message_format`:

```
feat({pbi_id}): {task_title}

{brief description}

Task: {task_id}
```

### Pull Request Title

```
[{task_id}] {task_description}
```

### Automation

When task transitions to Done:
```bash
git acp  # Add, commit, push
```

---

## Workflows

For step-by-step execution instructions:

- **Decompose Tasks**: `workflows/decompose-tasks/`
  - Breaks down approved PBI into tasks
  - Auto-approves all tasks (Agreed)
  - Creates task index + detail files

- **Execute Task**: `workflows/execute-task/`
  - Agreed → InProgress → InReview
  - Implements requirements with AI guidance
  - Runs tests and validations

---

## Related Sections

- [← Previous: Section 2 - PBI Management](./2-pbi-management.md)
- [Next: Section 4 - Testing Strategy →](./4-testing-strategy.md)
- [← Back to Index](../project-policy-index.md)

---

**Version:** 3.0 (Refactored - Policy Only)
**Last Updated:** 2025-10-21

**Note:** For detailed workflow execution, see `workflows/` directory.
