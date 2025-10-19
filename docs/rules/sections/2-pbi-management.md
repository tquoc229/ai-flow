# Section 2: PBI Management

[← Back to Index](mdc:../project-policy-index.md)

---

## 3.1 Overview

**Purpose:** Defines how PBIs (Product Backlog Items) are managed throughout their lifecycle.

**Key Concepts:**
- PBIs are the **primary units of work** that define product features and requirements
- Each PBI follows a defined workflow with clear state transitions
- All PBI changes are tracked and logged for full audit trail

---

## 3.2 Backlog Document Structure

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

## 3.3 PBI Workflow States

### State Definitions

| State | Description | What It Means |
|-------|-------------|---------------|
| `Proposed` | Initial state | PBI suggested but not yet approved |
| `Agreed` | Approved | Ready for implementation, tasks can be created |
| `InProgress` | Active work | Implementation underway, tasks being executed |
| `InReview` | Awaiting validation | Implementation complete, pending User review |
| `Done` | Completed | Accepted and merged, no further work needed |
| `Rejected` | Not accepted | Requires rework or has been deprioritized |

---

## 3.4 PBI State Transitions

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

### Transition 1: Creating a New PBI

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

### Transition 2: Approving PBI for Backlog

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

### Transition 3: Starting PBI Implementation

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
5. ✅ Begin working on first task (see Section 3: Task Management)

**Next Step:** Work through tasks one by one until all are Done

---

### Transition 4: Submitting PBI for Review

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

### Transition 5: Approving Completed PBI

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

### Transition 6: Rejecting PBI After Review

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

**Next Step:** Address feedback and reopen PBI

---

### Transition 7: Reopening Rejected PBI

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

### Transition 8: Deprioritizing PBI

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

## 3.5 PBI History Logging

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

---

## 3.6 PBI Detail Document Structure

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

[← Back to Index](mdc:../project-policy-index.md) | [Next: Task Management →](mdc:3-task-management.md)
