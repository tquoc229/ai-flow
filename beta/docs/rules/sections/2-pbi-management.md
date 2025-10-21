# Policy Section 2: PBI Management - Instructions

<critical>This document outlines the lifecycle, states, and management rules for Product Backlog Items (PBIs). Adherence is mandatory.</critical>
<critical>This is Section 2 of the AI Coding Agent Project Policy.</critical>

<workflow>

<step n="1" goal="Understand PBI Overview and Backlog Structure">
    <action>
        **3.1 Overview**
        - **Purpose:** Defines how PBIs are managed throughout their lifecycle.
        - **Key Concepts:** PBIs are the primary units of work, follow a defined workflow, and all changes are tracked.
    </action>
    <action>
        **3.2 Backlog Document Structure**
        - **Location:** `docs/delivery/backlog.md`
        - **Format:** A markdown table with columns: ID, Actor, User Story, Status, Conditions of Satisfaction (CoS).
        - **Core Principles:** Single source of truth, ordered by priority, unique IDs, and defined status values.
    </action>
</step>

<step n="2" goal="Learn PBI Workflow States">
    <action>
        **3.3 PBI Workflow States**
        The following states define the PBI lifecycle:
        | State | Description | What It Means |
        |---|---|---|
        | `Proposed` | New PBI idea | PBI mới được tạo (1-line user story). Chờ User duyệt ý tưởng. |
        | `PlanInReview` | Awaiting plan approval | AI_Agent đã phân tích và tạo file prd.md chi tiết. Chờ User duyệt kế hoạch này. |
        | `NeedsPlanRework` | Plan rejected | User từ chối kế hoạch trong prd.md, yêu cầu AI làm lại. |
        | `ReadyForTasks` | Plan approved, ready to decompose | User đã duyệt file prd.md. AI_Agent sẽ phân rã kế hoạch này thành Tasks. |
        | `InProgress` | Implementation in progress | AI_Agent đã tạo tasks và đang thực hiện implementation. |
        | `InReview` | Ready for final review | Tất cả tasks đã Done. Chờ User review và approve toàn bộ PBI. |
        | `Done` | Completed and approved | PBI đã hoàn thành và được User chấp nhận. |
        | `Rejected` | Failed review, needs rework | PBI bị reject sau review. Cần fix và submit lại. |
    </action>
    <action>
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
    </action>
</step>

<step n="3" goal="Execute PBI Transitions: Creation and Planning">
    <action>
        **Transition 1: Creating a New PBI ((none) → `Proposed`)**
        - **Who:** User or AI_Agent.
        - **Action:** Create a clear user story and CoS, assign a unique ID, add to `backlog.md`, and log history.
        - **Next Step:** Wait for User to approve the idea.
    </action>
    <action>
        **Transition 2: User Approves Idea / AI Generates Plan (`Proposed` → `PlanInReview`)**
        - **Trigger:** User manually changes status to `PlanInReview` in `backlog.md`.
        - **AI_Agent MUST:**
            1. Detect status change.
            2. Perform **Targeted Legacy Discovery** (search for relevant code, read ONLY that small set).
            3. Create PBI directory and `prd.md` detail document.
            4. Populate `prd.md`, prioritizing reuse of legacy code found.
            5. Create bidirectional links between `backlog.md` and `prd.md`.
            6. Log "Generated Plan for Review" in PBI history.
        - **Next Step:** Wait for User to review the detailed plan in `prd.md`.
    </action>
</step>

<step n="4" goal="Execute PBI Transitions: Task Decomposition and Rework">
    <action>
        **Transition 3: User Approves Plan / AI Decomposes Tasks (`PlanInReview` → `ReadyForTasks` → `InProgress`)**
        - **Trigger:** User manually changes status to `ReadyForTasks` in `backlog.md`.
        - **AI_Agent MUST:**
            1. Detect status change.
            2. Read the approved `prd.md`.
            3. Create `tasks.md` and all detailed task files (`<PBI-ID>-1.md`, etc.).
            4. Set all new tasks to `Proposed`.
            5. Update PBI status to `InProgress` in `backlog.md`.
            6. Log "Plan Approved, Tasks Decomposed" in PBI history.
        - **Next Step:** Task Management workflow begins.
    </action>
    <action>
        **Transition 3b: User Rejects Plan / AI Reworks Plan (`PlanInReview` → `NeedsPlanRework`)**
        - **Trigger:** User manually changes status to `NeedsPlanRework` in `backlog.md`.
        - **AI_Agent MUST:**
            1. Detect status change.
            2. Read User feedback.
            3. Revise the `prd.md` plan.
            4. Update PBI status back to `PlanInReview`.
            5. Log "Plan Reworked" in PBI history.
        - **Next Step:** Wait for User to review the updated plan.
    </action>
</step>

<step n="5" goal="Execute PBI Transitions: Submission and Approval">
    <action>
        **Transition 4: Submitting PBI for Review (`InProgress` → `InReview`)**
        - **Who:** AI_Agent.
        - **Pre-conditions:** ALL tasks for the PBI are `Done`, all CoS are met, all tests pass.
        - **AI_Agent MUST:** Verify pre-conditions, update PBI status to `InReview`, notify User, and log history.
        - **Next Step:** Wait for User review.
    </action>
    <action>
        **Transition 5: Approving Completed PBI (`InReview` → `Done`)**
        - **Who:** User only.
        - **AI_Agent MUST:** Record completion date, update status to `Done`, archive task docs, and log "Approved" in history.
        - **Next Step:** PBI is complete.
    </action>
</step>

<step n="6" goal="Execute PBI Transitions: Rejection and Deprioritization">
    <action>
        **Transition 6: Rejecting PBI After Review (`InReview` → `Rejected`)**
        - **Who:** User only.
        - **AI_Agent MUST:** Document rejection reasons from User, update status to `Rejected`, and log history.
        - **Next Step:** Wait for PBI to be reopened.
    </action>
    <action>
        **Transition 7: Reopening Rejected PBI (`Rejected` → `InProgress`)**
        - **Trigger:** User manually changes status to `InProgress`.
        - **AI_Agent MUST:** Read feedback, create new tasks for fixes, and log "Reopened" in history.
        - **Next Step:** Complete fixes and resubmit (Return to Transition 4).
    </action>
    <action>
        **Transition 8: Deprioritizing PBI (Any active state → `Proposed`)**
        - **Who:** User only.
        - **AI_Agent MUST:** Detect status change, document reason, pause any in-progress tasks, and log "Deprioritized" in history.
        - **Next Step:** PBI is returned to the backlog for later consideration.
    </action>
</step>

<step n="7" goal="Understand PBI History Logging">
    <action>
        **3.5 PBI History Logging**
        - **Location:** `docs/delivery/backlog.md` under "## PBI History".
        - **Purpose:** Track all changes for a complete audit trail.
        - **Required Fields:** Timestamp, PBI_ID, Action, Details, User.
        - **Action Logging:** Use plain language for actions like "Created", "Generated Plan for Review", "Approved", "Rejected", etc.
    </action>
    <action>
        **Example History:**
        ```markdown
        ## PBI History

        | Timestamp | PBI_ID | Action | Details | User |
        |---|---|---|---|---|
        | 2025-10-19 14:30:00 | PBI-1 | Created | Initial creation... | Julian |
        | 2025-10-19 15:00:00 | PBI-1 | Generated Plan... | AI created detailed plan... | ai-agent |
        | 2025-10-23 10:30:00 | PBI-1 | Approved | All CoS verified... | Julian |
        ```
    </action>
</step>

<step n="8" goal="Understand PBI Detail Document Structure">
    <action>
        **3.6 PBI Detail Document Structure (`prd.md`)**
        - **Location:** `docs/delivery/<PBI-ID>/prd.md`.
        - **Purpose:** The source of truth for PBI context, requirements, and high-level planning.
        - **Creation:** Created automatically when a PBI moves from `Proposed` → `PlanInReview`.
    </action>
    <action>
        **Required Sections:**
        - **YAML Frontmatter:** status, priority, created, updated, estimated_hours.
        - **Context:** Why, Current Situation, Desired State.
        - **User Story:** As a..., I want..., So that...
        - **Requirements:** Functional and Non-Functional.
        - **Technical Approach:** Architecture, **Legacy Discovery Findings**, Key Components, Tech Stack.
        - **Implementation Plan:** High-level phases and steps.
        - **Testing Strategy:** Unit, Integration, and E2E tests.
        - **Success Criteria:** Checklist for completion.
        - **References & Notes**.
    </action>
    <action>
        **CRITICAL SECTION: Legacy Discovery Findings**
        - The AI_Agent MUST fill this section after performing a targeted search for reusable code.
        - The Implementation Plan must prioritize modifying legacy code found here.
    </action>
</step>

<step n="9" goal="Navigate Between Sections">
    <action>Use these links to navigate the policy documents.</action>
    <action>- [← Back to Index](../project-policy-index.md)</action>
    <action>- [← Previous: Section 1 - Fundamentals](./1-fundamentals.md)</action>
    <action>- [Next: Section 3 - Task Management →](./3-task-management.md)</action>
</step>

</workflow>