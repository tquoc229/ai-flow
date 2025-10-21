# Policy Section 3: Task Management - Instructions

<critical>This document defines how tasks, the smallest units of work, are created, executed, and tracked. Adherence is mandatory.</critical>
<critical>This is Section 3 of the AI Coding Agent Project Policy.</critical>

<workflow>

<step n="1" goal="Understand Task Overview and Document Structure">
    <action>
        **4.1 Overview**
        - **Purpose:** Defines how tasks are managed as atomic units of work within PBIs.
        - **Key Concepts:** Tasks follow a defined workflow, changes are synchronized, and only one task per PBI is `InProgress` at a time.
    </action>
    <action>
        **4.2 Task Document Structure**
        - **Location:** `docs/delivery/<PBI-ID>/`
        - **Naming:** `tasks.md` for the list, `<PBI-ID>-<TASK-ID>.md` for details.
        - **Required Sections:** Task files must include Goal, Context, Status History, Requirements, Implementation Steps, Files to Modify/Create, Testing, Success Criteria, and References.
        - **Core Principles:** Each task has a dedicated file, files are created immediately, naming is strict, and bidirectional links are maintained.
    </action>
</step>

<step n="2" goal="Learn Task Workflow States">
    <action>
        **4.3 Task Workflow States**
        The Task lifecycle is defined by the states listed in `task.states` in `config.yaml`. The standard states are:
        | State | Description |
        |---|---|
        | `Proposed` | Defined but not yet approved by User. |
        | `Agreed` | Approved by User, ready for implementation. |
        | `InProgress` | AI_Agent is actively implementing the task. |
        | `Review` | Implementation complete, pending User review. |
        | `Done` | User approved, changes merged, task closed. |
        | `Blocked` | Cannot proceed due to an external issue. |
        | `Cancelled` | Task cancelled and will not be worked on. |
    </action>
    <action>
        **4.4 Workflow Diagram:**
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
    </action>
</step>

<step n="3" goal="Execute Task Transitions: Creation and Starting Work">
    <action>
        **Transition 1: Creating and Auto-Approving a Task ((none) → `Proposed` → `Agreed`)**
        - **When:** AI decomposes an approved PBI plan.
        - **Action:** AI creates all task files and automatically sets their status to `Agreed`. This is because the User has already approved the high-level plan.
        - **AI MUST:** Create the task file, add it to the index, populate all sections, set status to `Agreed` in the task file, and log "Created" and "Auto-Approved" in the history.
    </action>
    <action>
        **Transition 2: Starting Work on Task (`Agreed` → `InProgress`)**
        - **Pre-conditions:** No other tasks for this PBI are `InProgress`.
        - **AI MUST:** Verify the concurrency limit, create a feature branch, update status to `InProgress` in the task file, log history, and begin implementation.
        - **CRITICAL:** If another task is `InProgress`, STOP and report to the User.
    </action>
</step>

<step n="4" goal="Execute Task Transitions: Submission and Approval">
    <action>
        **Transition 3: Submitting Task for Review (`InProgress` → `Review`)**
        - **Pre-conditions:** All requirements implemented, all tests passing, code follows standards.
        - **AI MUST:** Verify tests pass, update task documentation, update status to `Review` in the task file, log history, create a pull request, and notify the User.
    </action>
    <action>
        **Transition 4: Approving Completed Task (`Review` → `Done`)**
        - **Who:** User only.
        - **AI MUST:**
            1. **Review Next Tasks for Relevance** using the Obsolescence Criteria.
            2. Ask User to confirm removal/modification of any obsolete tasks.
            3. Merge changes to the main branch.
            4. Update status to `Done` in the task file and log history.
            5. Execute version control workflow (e.g., `git acp`).
    </action>
</step>

<step n="5" goal="Execute Task Transitions: Rework and Updates">
    <action>
        **Transition 5: Rejecting Task (Needs Rework) (`Review` → `InProgress`)**
        - **Who:** User only.
        - **AI MUST:** Document rejection reasons from User feedback, update status back to `InProgress` in the task file, log history, and resume work to fix the issues.
    </action>
    <action>
        **Transition 6: Updating Task Requirements (`Review` → `InProgress`)**
        - **When:** User identifies significant requirement changes during review.
        - **AI MUST:** Document the changes in the task file, update status to `InProgress`, log history, and resume work to implement the new requirements.
    </action>
</step>

<step n="6" goal="Execute Task Transitions: Blocking and Unblocking">
    <action>
        **Transition 7: Blocking a Task (`InProgress` → `Blocked`)**
        - **When:** Progress is prevented by an external issue.
        - **AI MUST:** Stop work, document the specific blocking reason in the task file, update status to `Blocked`, log history, and notify the User immediately.
    </action>
    <action>
        **Transition 8: Unblocking a Task (`Blocked` → `InProgress`)**
        - **When:** The blocking issue is resolved.
        - **AI MUST:** Document the resolution, update status back to `InProgress` in the task file, log history, and resume work.
    </action>
</step>

<step n="7" goal="Understand Task Status Management (Single Source of Truth)">
    <action>
        **4.5 Task Status Management (Single Source of Truth)**
        - **Authoritative Source:** The task detail file (`<PBI-ID>-<TASK-ID>.md`) is the ONLY place where status is manually changed.
        - **Auto-Generated Index:** The task index file (`tasks.md`) is automatically regenerated from the detail files by a pre-commit hook.
        - **Process:** AI updates the status in the task file only. The pre-commit hook handles updating the index, ensuring they are always synchronized within the same commit.
        - **Benefit:** This eliminates synchronization errors and reduces cognitive load.
    </action>
</step>

<step n="8" goal="Adhere to Concurrency, History, and Validation Rules">
    <action>
        **4.6 Task Concurrency Limit**
        - **MANDATORY RULE:** Only {task.max_concurrent} task(s) per PBI may be `InProgress` at any time to maintain focus and prevent conflicts.
        - **Enforcement:** Before starting a task, the AI must check for other `InProgress` tasks for the same PBI.
    </action>
    <action>
        **4.7 Task History Logging**
        - **Location:** In the task markdown file under `## Status History`.
        - **Purpose:** To track all changes for a complete audit trail.
        - **Action:** Use plain language for actions like "Created", "Started Work", "Blocked", etc.
    </action>
    <action>
        **4.8 Task Validation Rules**
        - **Core Rules:** Tasks must be associated with a PBI, follow the workflow, have complete documentation, and maintain file consistency.
        - **Error Prevention:** If an error is detected (e.g., file inaccessible), STOP, report to the User, wait for guidance, apply the fix, and then proceed.
    </action>
</step>

<step n="9" goal="Learn Task Obsolescence Criteria">
    <action>
        **4.9 Task Obsolescence Criteria**
        A task is OBSOLETE and should be proposed for removal if it meets ANY of these 4 criteria.
    </action>
    <action>
        **Criterion 1: Already Satisfied by Previous Work**
        - The task's requirements have already been fully satisfied by a previous task.
    </action>
    <action>
        **Criterion 2: Superseded by Implementation Approach**
        - The actual implementation path diverged from the original plan, making the task unnecessary (e.g., a different architecture was used).
    </action>
    <action>
        **Criterion 3: External Dependency Now Provides Functionality**
        - A third-party library or service added in another task now provides the functionality this task was meant to build.
    </action>
    <action>
        **Criterion 4: PBI Requirements Changed**
        - The User has explicitly changed the PBI requirements, removing the need for this task.
    </action>
</step>

<step n="10" goal="Follow the Task Obsolescence Evaluation Process">
    <action>
        **When to Evaluate:** After completing any task, when PBI requirements change, or when discovering existing functionality.
    </action>
    <action>
        **Evaluation Process for AI_Agent:**
        1. **List Remaining Tasks:** Get all tasks that are not `Done` or `InProgress`.
        2. **Check Criteria:** For each task, check against the 4 obsolescence criteria.
        3. **Document Findings:** For each flagged task, create a report with the task, the criterion it met, and the reason.
        4. **Present to User:** Summarize the findings and ask the User to confirm removal or modification.
        5. **Execute User Decision:** Remove, modify, or keep the task as per the User's decision and log the action.
    </action>
    <action>
        **Anti-Patterns (What NOT to Mark Obsolete):**
        - Do not mark a task as obsolete just because it is hard, will take time, or could be done later. These are not valid reasons for obsolescence.
    </action>
</step>

<step n="11" goal="Follow Version Control and Indexing Rules">
    <action>
**Version Control for Task Completion**
        - **Commit Message Format:** `{automation.commit_message_format}` (e.g., `feat(6): 6-1 Define and implement Circuit Breaker state machine`).
        - **Pull Request Title:** `[<task-id>] <task-description>`.
        - **Automation:** When a task is `Done`, the `git acp` command should be used to stage, commit, and push changes.
    </action>
    <action>
        **4.10 Task Index File Structure (`tasks.md`)**
        - **Purpose:** Provides a consolidated, auto-generated view of all tasks for a PBI.
        - **Format:** A markdown table with columns: Task ID, Name (linked to detail file), Status, and Description.
        - **Restrictions:** The table must only contain these four columns and task information. No extra notes or unlinked entries.
    </action>
</step>

<step n="12" goal="Navigate Between Sections">
    <action>Use these links to navigate the policy documents.</action>
    <action>- [← Back to Index](../project-policy-index.md)</action>
    <action>- [← Previous: Section 2 - PBI Management](./2-pbi-management.md)</action>
    <action>- [Next: Section 4 - Testing Strategy →](./4-testing-strategy.md)</action>
</step>

</workflow>