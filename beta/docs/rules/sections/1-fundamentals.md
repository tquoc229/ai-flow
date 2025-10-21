# Policy Section 1: Fundamentals - Instructions

<critical>This document outlines the fundamental principles, rules, and automation flows that govern all development work. Adherence is mandatory.</critical>
<critical>This is Section 1 of the AI Coding Agent Project Policy.</critical>

<workflow>

<step n="1" goal="Understand Introduction, Roles, and Architecture">
    <action>
        **1. Introduction**
        This policy provides a single, authoritative, machine-readable source of truth for AI coding agents and humans. It ensures all work is governed by clear, unambiguous rules and workflows.
    </action>
    <action>
        **1.1 Actors**
        | Actor | Role | Responsibilities |
        |---|---|---|
        | **User** | Decision Maker | Defines requirements, prioritizes work, approves changes, accountable for all code |
        | **AI_Agent** | Executor | Implements User's instructions precisely as defined by PBIs and tasks |
    </action>
    <action>
        **1.2 Architectural Compliance**
        - ✅ All tasks must be explicitly defined and agreed upon before implementation.
        - ✅ All code changes must be associated with a specific task.
        - ✅ All PBIs must be aligned with the PRD when applicable.
    </action>
</step>

<step n="2" goal="Learn the Core Principles">
    <action>These principles are MANDATORY and govern all work.</action>
    <action>
        **🎯 Task-Driven Development**
        - ❌ NO code changes without an agreed-upon task.
        - ✅ Every change must have explicit authorization.
    </action>
    <action>
        **📦 PBI Association**
        - ❌ NO task creation without an associated PBI.
        - ✅ All work traces back to a Product Backlog Item.
    </action>
    <action>
        **📖 PRD Alignment**
        - ✅ If a PRD exists, all PBI features must align with its scope.
        - ✅ Sense-check all features against PRD requirements.
    </action>
    <action>
        **👤 User Authority**
        - ✅ User is the sole decider for scope and design.
        - ✅ User retains responsibility for all code changes (even if AI implements).
    </action>
    <action>
        **🚫 Prohibition of Unapproved Changes**
        - ❌ EXPRESSLY PROHIBITED: Any changes outside explicit task scope.
        - ✅ Identify scope creep → roll back → create new task.
    </action>
    <action>
        **🔄 Task Status Synchronization**
        - ✅ Status in `1-tasks.md` MUST match status in individual task file.
        - ✅ Update BOTH locations immediately on status change.
    </action>
    <action>
        **📄 Controlled File Creation**
        - ❌ DO NOT create files outside defined structures (PBIs, tasks, source code).
        - ✅ Get explicit User confirmation before creating any standalone files.
    </action>
</step>

<step n="3" goal="Follow Development Best Practices">
    <action>
        **📚 External Package Research**
        To avoid hallucinations when using external packages:
        1. **Research first:** Use web search to find official documentation.
        2. **Create guide:** For each package, create `tasks/<task-id>-<package>-guide.md`.
        3. **Document:** Include date stamp, link to docs, API examples, and code snippets.
    </action>
    <action>
        **⚙️ Task Granularity**
        - ✅ Tasks should be as small as practicable.
        - ✅ Must still represent a cohesive, testable unit.
        - ✅ Break down complex features into multiple smaller tasks.
    </action>
    <action>
        **🔁 Don't Repeat Yourself (DRY)**
        - ✅ Information should exist in ONE location.
        - ✅ Task details go in dedicated task files.
        - ✅ PBI documents reference tasks, they don't duplicate them.
    </action>
    <action>
        **🔢 Use Constants for Values**
        - ✅ Any value used more than once must be defined as a named constant.
        - ❌ **BAD:** `for (let i = 0; i < 10; i++)`
        - ✅ **GOOD:** `const NUM_WEBSITES = 10; for (let i = 0; i < NUM_WEBSITES; i++)`
    </action>
    <action>
        **📖 Technical Documentation**
        - ✅ For any PBI creating/modifying APIs or interfaces, create documentation in `docs/technical/` or inline.
        - ✅ Include API usage, contracts, integration guides, config options, and error handling.
    </action>
    <action>
        **🔁 Principle of Hierarchical Planning (PBI vs. Task)**
        - ✅ PBI Plan (`prd.md`): High-level, multi-task strategy.
        - ✅ Task Plan (`<ID>-<TaskNum>.md`): Detailed, step-by-step, executable actions.
        - ✅ The AI_Agent decomposes the high-level PBI plan into detailed Tasks.
    </action>
    <action>
        **♻️ Legacy Code Prioritization**
        - ✅ The AI_Agent must ALWAYS prioritize searching for and leveraging existing code.
        - ❌ PROHIBITED: Writing new code for functionality that already exists.
        - ✅ Implementation plans must state which legacy assets will be reused.
    </action>
</step>

<step n="4" goal="Understand AI Agent Automation Principles">
    <action>
        **Purpose:** Defines how AI agents interact with the workflow system to ensure synchronization and integrity.
    </action>
    <action>
        **Workflow Interaction Principles - The AI_Agent MUST automatically ensure:**
        1. **Automatic Synchronization:** Status consistency, atomic file updates, and history log maintenance.
        2. **Workflow State Management:** Follow defined state transitions, validate pre-conditions, execute actions, and log all changes.
        3. **Integrity Verification:** Verify task existence, validate transitions, check synchronization, and report inconsistencies.
    </action>
    <action>
        **Automation Safeguards - The AI_Agent:**
        - ✅ **NEVER** makes status changes without updating both locations, creates tasks without files, modifies files outside task scope, or proceeds if validation fails.
        - ✅ **ALWAYS** validates before acting, updates atomically, logs all changes, reports inconsistencies, and asks the User when uncertain.
    </action>
</step>

<step n="5" goal="Learn the AI Agent's Automated Task Management Flow">
    <action>When working with tasks, the AI_Agent AUTOMATICALLY performs these flows.</action>
    <action>
        **1. Task Initiation (Proposed → Agreed)**
        The AI_Agent MUST:
        - Create task file: `<PBI-ID>-<TASK-ID>.md`
        - Link in tasks index
        - Populate all required sections
        - Update status in BOTH locations
        - Log creation in history
    </action>
    <action>
        **2. Task Execution (Agreed → InProgress)**
        The AI_Agent MUST verify no other `InProgress` tasks for the same PBI and that status matches, then update status in both locations and log history.
    </action>
    <action>
        **3. Task Completion (InProgress → Review → Done)**
        - The AI_Agent verifies work, then updates status to `Review` in both locations and logs it.
        - After User approval (`Review` → `Done`), the AI_Agent updates both locations to `Done`, executes version control workflow, and logs completion.
    </action>
</step>

<step n="6" goal="Learn the AI Agent's Automated PBI Management Flow">
    <action>When working with PBIs, the AI_Agent AUTOMATICALLY performs these flows.</action>
    <action>
        **1. PBI Creation (→ Proposed)**
        The AI_Agent verifies unique ID and required fields, then logs creation.
    </action>
    <action>
        **2. PBI Plan Generation (Proposed → PlanInReview)**
        After User approval, the AI_Agent MUST:
        - Scan codebase for reusable assets.
        - Create PBI detail document (`<PBI-ID>/prd.md`).
        - Populate sections using discovery findings.
        - Update status to `PlanInReview` and log the action.
    </action>
    <action>
        **3. PBI Task Decomposition (PlanInReview → InProgress)**
        After User approves the plan, the AI_Agent MUST:
        - Decompose the plan into task files.
        - Set all new tasks to `Proposed`.
        - Update PBI status to `InProgress` and log the action.
    </action>
    <action>
        **4. PBI Review (InProgress → InReview)**
        After all tasks are `Done`, the AI_Agent verifies completion, updates PBI status, and notifies the user for review.
    </action>
</step>

<step n="7" goal="Understand AI Agent Validation and Error Handling">
    <action>The AI_Agent performs these checks AUTOMATICALLY at each interaction.</action>
    <action>
        **Automatic Validation Checkpoints:**
        - **Before ANY code change:** Verify linked PBI/Task, check `InProgress` status, confirm scope.
        - **Before ANY status change:** Validate transition is allowed, verify pre-conditions.
        - **After ANY file update:** Verify synchronization, confirm history is logged.
    </action>
    <action>
        **Error Detection and Recovery:**
        The AI_Agent MUST detect Synchronization Errors, Workflow Violations, and Integrity Issues.
        **Recovery Actions:**
        1. ⛔ STOP all work immediately.
        2. 📢 Report the error to the User with details.
        3. ⏸️ Wait for User guidance.
        4. ✅ Apply fix and verify resolution.
    </action>
</step>

<step n="8" goal="Adhere to Governance Rules">
    <action>
        **2.3 PRD Alignment Check**
        - ✅ MUST check all PBIs for alignment with PRD.
        - ⚠️ MUST raise any discrepancies with User.
    </action>
    <action>
        **2.4 Integrity and Sense Checking**
        - ✅ All data must be sense-checked for consistency and accuracy.
    </action>
    <action>
        **2.5 Scope Limitations**
        - ❌ NO gold plating or scope creep.
        - ✅ All work scoped to a specific task. Improvements must be proposed as separate tasks.
    </action>
    <action>
        **2.6 Change Management Rules**
        Before ANY code change, identify the task and verify scope. If a User requests a change without a task, DO NOT proceed; discuss creating a new task first.
    </action>
</step>

<step n="9" goal="Navigate Between Sections">
    <action>Use these links to navigate the policy documents.</action>
    <action>- [← Back to Index](../project-policy-index.md)</action>
    <action>- [Next: Section 2 - PBI Management →](./2-pbi-management.md)</action>
</step>

</workflow>