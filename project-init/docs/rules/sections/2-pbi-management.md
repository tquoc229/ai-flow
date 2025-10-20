[← Back to Index](../project-policy-index.md) | [← Previous: Section 1](./1-fundamentals.md)

---

# Section 2: PBI Management

**Contains:** PBI Overview, Backlog Structure, Workflow States, State Transitions, History Logging, PBI Detail Document Structure

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
| 1  | User  | As a user... | PlanInReview | - Criterion 1<br>- Criterion 2 |
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
| `Proposed` | New PBI idea | PBI mới được tạo (1-line user story). Chờ User duyệt ý tưởng. |
| `PlanInReview` | Awaiting plan approval | AI_Agent đã phân tích và tạo file prd.md chi tiết. Chờ User duyệt kế hoạch này. |
| `NeedsPlanRework` | Plan rejected | User từ chối kế hoạch trong prd.md, yêu cầu AI làm lại. |
| `ReadyForTasks` | Plan approved, ready to decompose | User đã duyệt file prd.md. AI_Agent sẽ phân rã kế hoạch này thành Tasks. |
| `InProgress` | Implementation in progress | AI_Agent đã tạo tasks và đang thực hiện implementation. |
| `InReview` | Ready for final review | Tất cả tasks đã Done. Chờ User review và approve toàn bộ PBI. |
| `Done` | Completed and approved | PBI đã hoàn thành và được User chấp nhận. |
| `Rejected` | Failed review, needs rework | PBI bị reject sau review. Cần fix và submit lại. |

---

### 3.4 PBI State Transitions

**Workflow Diagram:**

```
         [Proposed]
             ↓
      [PlanInReview] ←─────────┐
         ↓       ↓             │
   [ReadyForTasks] [NeedsPlanRework]
         ↓
    [InProgress] ←──────────┐
         ↓                  │
     [InReview]             │
         ↓                  │
      [Done]           [Rejected]
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

**Next Step:** Wait for User to approve the PBI idea (User will trigger by updating status)

---

#### Transition 2: User Approves PBI Idea / AI Generates Plan

**When:** User approves the 1-line PBI idea in `backlog.md`

**State Change:** `Proposed` → `PlanInReview`

**Who Can Do This:** User (triggers), AI_Agent (executes)

**Before (User):**
* User reviews the 1-line PBI in `backlog.md` and decides to approve it
* User manually changes PBI status from `Proposed` to `PlanInReview` (this triggers AI to generate plan)

**AI_Agent MUST Do:**
1.  ✅ **Detect** PBI status changed to `PlanInReview`
2.  ✅ **Perform Targeted Legacy Discovery:**
    * **a. Generate Keywords:** Based on the PBI `User Story`, generate a list of relevant search keywords (e.g., "profile", "user", "functionName")
    * **b. Execute Search:** Use code search tools (e.g., `ripgrep`, vector search) to find a *small set* of the **MOST RELEVANT** files or code chunks
    * **c. Read Context:** Read **ONLY** that small, relevant set of files. **DO NOT read the entire codebase.** This is critical to conserve tokens and prevent overload
3.  ✅ **Create directory:** `docs/delivery/<PBI-ID>/`
4.  ✅ **Create PBI detail document:** `docs/delivery/<PBI-ID>/prd.md` using Template (Section 3.6)
5.  ✅ **Perform Analysis & Populate:** Auto-fill all sections in the `prd.md` file
    * **CRITICAL:** The `Legacy Discovery Findings` section **MUST** be populated based on the results from the *targeted* discovery (Step 2c)
    * The high-level `Implementation Plan` must prioritize *modifying* legacy code over *creating* new code, based on these findings
6.  ✅ **Set up bidirectional links:**
    * In `backlog.md`: Update the 1-line PBI to link to the new `prd.md` file
    * In `prd.md`: Add a link back to `backlog.md`
7.  ✅ **Log in PBI history:**
    * Action: "Generated Plan for Review"
    * Details: "AI performed Targeted Discovery and generated detailed PBI plan, awaiting User approval"
    * User: ai-agent

**Next Step:** PBI remains in `PlanInReview` state. Wait for User to review the detailed plan in `prd.md` and either approve (→ `ReadyForTasks`) or reject (→ `NeedsPlanRework`)

---

#### Transition 3: User Approves Plan / AI Decomposes Tasks

**When:** User reviews and approves the detailed plan in prd.md

**State Change:** `PlanInReview` → `ReadyForTasks` → `InProgress`

**Who Can Do This:** User (triggers), AI_Agent (executes)

**Before (User):**

- User reviews `prd.md` and performs any necessary edits
- User manually changes PBI status from `PlanInReview` to `ReadyForTasks` in `backlog.md` (this triggers AI to decompose tasks)

**AI_Agent MUST Do:**

1. ✅ **Detect** PBI status changed to `ReadyForTasks`
2. ✅ **Read** the approved `prd.md` file
3. ✅ **Create task list file:** `docs/delivery/<PBI-ID>/tasks.md` (see Section 4.10 for structure)
4. ✅ **Decompose Plan:** Read the high-level `Implementation Plan` from `prd.md` and break it down into detailed Tasks
5. ✅ **Create ALL Task Files:** Create all detailed task files (e.g., `<PBI-ID>-1.md`, `<PBI-ID>-2.md`, etc.) following Section 4.2 template
6. ✅ **Set all new tasks to** `Proposed` (ready for User to review and approve each task)
7. ✅ **Update PBI status to** `InProgress` in `backlog.md` (signals PBI implementation has officially started)
8. ✅ **Log in PBI history:**
   - Action: "Plan Approved, Tasks Decomposed"
   - Details: "User approved plan. AI decomposed into [X] tasks"
   - User: ai-agent

**Next Step:** Task Management workflow (Section 4) begins. User approves first Task (`Proposed` → `Agreed`) for AI to start coding

---

#### Transition 3b: User Rejects Plan / AI Reworks Plan

**When:** User reviews plan in prd.md and finds it needs significant changes

**State Change:** `PlanInReview` → `NeedsPlanRework`

**Who Can Do This:** User (triggers), AI_Agent (executes)

**Before (User):**

- User reviews `prd.md` and identifies issues with the plan
- User provides specific feedback on what needs to be changed
- User manually changes PBI status from `PlanInReview` to `NeedsPlanRework` in `backlog.md`

**AI_Agent MUST Do:**

1. ✅ **Detect** PBI status changed to `NeedsPlanRework`
2. ✅ **Read User feedback** from comments or updated requirements
3. ✅ **Revise the plan:** Update `prd.md` based on User feedback
4. ✅ **Re-perform Targeted Discovery** if needed (if scope or approach changed significantly)
5. ✅ **Update PBI status back to** `PlanInReview` in `backlog.md`
6. ✅ **Log in PBI history:**
   - Action: "Plan Reworked"
   - Details: "AI revised plan based on User feedback"
   - User: ai-agent

**Next Step:** PBI goes back to `PlanInReview` state. Wait for User to review the updated plan again

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

**State Change:** `InReview` → `Rejected`, `PlanInReview` → `NeedsPlanRework`

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

**Who Can Do This:** User (triggers), AI_Agent (executes)

**Before (User):**

- User reviews rejection feedback
- User decides to reopen the PBI for fixes
- User manually changes PBI status from `Rejected` to `InProgress` in `backlog.md`

**AI_Agent MUST Do:**

1. ✅ **Detect** PBI status changed back to `InProgress`
2. ✅ **Read rejection feedback** and understand what needs to be fixed
3. ✅ **Create new tasks** for required changes (if needed)
4. ✅ **Update PBI detail document** with changes made or planned changes
5. ✅ **Log in PBI history:**
   - Action: "Reopened"
   - Details: "PBI reopened to address rejection feedback. Created [X] new tasks"
   - User: ai-agent

**Next Step:** Complete fixes and resubmit for review (return to Transition 4)

---

#### Transition 8: Deprioritizing PBI

**When:** PBI needs to be postponed or moved down in priority

**State Change:** Any active state (`PlanInReview`, `NeedsPlanRework`, `ReadyForTasks`, `InProgress`) → `Proposed`

**Who Can Do This:** User only

**Before (User):**

- User decides this work should be postponed
- User provides reason for deprioritization
- User manually changes PBI status to `Proposed` in `backlog.md`

**AI_Agent MUST Do:**

1. ✅ **Detect** PBI status changed to `Proposed` from an active state
2. ✅ **Document reason** for deprioritization (from User comments)
3. ✅ **Pause any in-progress tasks** (update their status to `Paused` or similar to document the pause)
4. ✅ **Move PBI to lower priority** in backlog (reorder if needed)
5. ✅ **Log in PBI history:**
   - Action: "Deprioritized"
   - Details: "Reason for deprioritization: [User's reason]"
   - User: User name who deprioritized

**Next Step:** PBI goes back to `Proposed` state, can be re-approved later (return to Transition 2)

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
| Generate plan for approval | "Generated Plan for Review" |
| Plan needs rework | "Plan Reworked" |
| Plan approved, tasks decomposed | "Plan Approved, Tasks Decomposed" |
| Start implementing tasks | "Started Implementation" |
| Submit for review | "Submitted for Review" |
| Approve completed PBI | "Approved" |
| Reject PBI after review | "Rejected" |
| Reopen after rejection | "Reopened" |
| Lower priority | "Deprioritized" |

**Example History:**

```markdown
## PBI History

| Timestamp | PBI_ID | Action | Details | User |
|-----------|--------|--------|---------|------|
| 2025-10-19 14:30:00 | PBI-1 | Created | Initial creation of user authentication PBI | Julian |
| 2025-10-19 15:00:00 | PBI-1 | Generated Plan for Review | AI performed Targeted Discovery and created detailed plan in prd.md | ai-agent |
| 2025-10-19 16:00:00 | PBI-1 | Plan Approved, Tasks Decomposed | User approved plan. AI decomposed into 5 tasks | ai-agent |
| 2025-10-20 09:15:00 | PBI-1 | Started Implementation | Working on task 1-1 | ai-agent |
| 2025-10-22 16:45:00 | PBI-1 | Submitted for Review | All 5 tasks completed, tests passing | ai-agent |
| 2025-10-23 10:30:00 | PBI-1 | Approved | All CoS verified and accepted, merged to main | Julian |
```

**More Examples:**

```markdown
## PBI History

| Timestamp | PBI_ID | Action | Details | User |
|-----------|--------|--------|---------|------|
| 2025-10-19 10:00:00 | PBI-2 | Created | Email notification system proposal | Julian |
| 2025-10-19 10:30:00 | PBI-2 | Generated Plan for Review | AI created initial plan for email notification system | ai-agent |
| 2025-10-19 11:00:00 | PBI-2 | Plan Reworked | User requested changes to architecture approach | ai-agent |
| 2025-10-19 14:00:00 | PBI-2 | Plan Approved, Tasks Decomposed | User approved revised plan. AI decomposed into 3 tasks | ai-agent |
| 2025-10-20 14:00:00 | PBI-2 | Started Implementation | Working on email service integration | ai-agent |
| 2025-10-24 15:00:00 | PBI-2 | Submitted for Review | Implementation complete, ready for testing | ai-agent |
| 2025-10-25 10:00:00 | PBI-2 | Rejected | Email templates need redesign per UX feedback | Julian |
| 2025-10-25 11:00:00 | PBI-2 | Reopened | PBI reopened to address rejection feedback. Created 1 new tasks | ai-agent |
| 2025-10-26 16:00:00 | PBI-2 | Submitted for Review | Templates redesigned and approved | ai-agent |
| 2025-10-27 09:30:00 | PBI-2 | Approved | Feature complete and deployed | Julian |
```

---

### 3.6 PBI Detail Document Structure

**Location:** `docs/delivery/<PBI-ID>/prd.md`

**Purpose:**
- Serves as the primary source of truth for all PBI context, requirements, and high-level planning.
- Documents the problem space, desired state, and strategic approach.
- Provides the high-level plan for the AI_Agent to decompose into granular tasks.

**Required Sections:**

```markdown
---
status: todo
priority: medium
created: YYYY-MM-DD
updated: YYYY-MM-DD
estimated_hours: X
---

# [PBI Title/Name]

## Context

**Why do we need this?**
[Explain the problem or opportunity]

**Current Situation:**
[What exists now]

**Desired State:**
[What we want]

## User Story

As a [user type]
I want [capability]
So that [benefit]

**Example Scenario:**

```
Given [precondition]
When [user action]
Then [expected outcome]
```

## Requirements

### Functional Requirements

- [ ] REQ-1: [Specific requirement]
- [ ] REQ-2: [Specific requirement]
- [ ] REQ-3: [Specific requirement]

### Non-Functional Requirements

- [ ] Performance: [e.g., Response time < 200ms]
- [ ] Security: [e.g., Requires authentication]
- [ ] Scalability: [e.g., Support 1000 concurrent users]

## Technical Approach

**Architecture:**

```
[Describe or diagram the architecture]
```
---

---
### **Legacy Discovery Findings**
[**AI_Agent MUST FILL THIS SECTION.** Report on existing files, functions, APIs, or components relevant to this PBI, adhering to the `Legacy Code Prioritization` principle.]

- **File:** `path/to/relevant/file.ts`
    - **Function/API:** `functionName()`
    - **Notes:** [Will be reused/extended for...]
- **Component:** `path/to/component.vue`
    - **Notes:** [Will be reused...]

---

**Key Components (To build/modify):**
[List new components to build OR existing components to modify, based on "Discovery Findings" above.]

1. Component A: [Purpose]
2. Component B: [Purpose]
3. Component C: [Purpose]

**Technology Stack:**

- Frontend: [technologies]
- Backend: [technologies]
- Database: [technologies]
- External Services: [APIs, libraries]

## Implementation Plan

### Phase 1: [Phase Name]

1. Step 1: [Description]

   - Files: [list files to create/modify]
   - Estimated: [time]

2. Step 2: [Description]
   - Files: [list files]
   - Estimated: [time]

### Phase 2: [Phase Name]

...

## Testing Strategy

**Unit Tests:**

- Test case 1
- Test case 2

**Integration Tests:**

- Test scenario 1
- Test scenario 2

**E2E Tests:**

- User flow 1
- User flow 2

## Success Criteria

- [ ] All functional requirements met
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Performance benchmarks met

## References

- [Link to design doc]
- [Link to API spec]
- [Link to similar implementation]

## Notes

[Any additional notes, constraints, or considerations]


```

**Document Linking:**

**From PBI detail to backlog:**
```markdown
**Backlog Entry:** [View in Backlog](../backlog.md#user-content-PBI-<ID>)
```

**From backlog to PBI detail:**
```markdown
| 1 | User | As a user... | PlanInReview | [View Details](1/prd.md) |
```

**Creation and Ownership:**
- ✅ Created automatically when PBI moves from `Proposed` → `PlanInReview`
- ✅ Maintained by implementing team member
- ✅ Reviewed during PBI review process
- ✅ Updated throughout PBI lifecycle

---

## Navigation

- [← Back to Index](../project-policy-index.md)
- [← Previous: Section 1 - Fundamentals](./1-fundamentals.md)
- [Next: Section 3 - Task Management →](./3-task-management.md)

---

**End of Section 2**
