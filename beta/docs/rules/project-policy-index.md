# AI Coding Agent Project Policy - Instructions

<critical>This document is the single, authoritative, machine-readable source of truth for all AI coding agents and human developers. All work is governed by these rules.</critical>
<critical>Version: 2.1 (Modular) | Last Updated: 2025-10-20</critical>

<workflow>

<step n="1" goal="Understand Core Principles">
    <action>Review the fundamental rules governing all project work.</action>
    <action>
**Task-Driven Development:**
- NO code changes without an agreed-upon task.
- Every change must have explicit authorization.
- All work traces back to a Product Backlog Item (PBI).
    </action>
    <action>
**User Authority:**
- The User is the sole decider for scope and design.
- The User retains responsibility for all code changes.
    </action>
    <action>
**Task Status Management (Single Source of Truth):**
- The task detail file is the authoritative source for status.
- The task index (`tasks.md`) is auto-generated and should not be edited manually.
    </action>
    <action>
**Prohibited Actions:**
- NO changes outside explicit task scope.
- NO file creation without User confirmation.
- NO gold plating or scope creep.
    </action>
</step>

<step n="2" goal="Learn the Documentation Structure">
    <action>This policy is organized into 5 focused sections plus this index.</action>
    <action>
**[Section 1: Fundamentals](./sections/1-fundamentals.md)**
- **Content:** Core principles, roles, architectural compliance, automation rules, change management.
- **Usage:** Onboarding, clarifying rules, understanding fundamentals.
    </action>
    <action>
**[Section 2: PBI Management](./sections/2-pbi-management.md)**
- **Content:** PBI lifecycle, states, transitions, and document structure.
- **Usage:** Creating, managing, and tracking PBIs.
    </action>
    <action>
**[Section 3: Task Management](./sections/3-task-management.md)**
- **Content:** Task lifecycle, states, transitions, concurrency, and validation.
- **Usage:** Creating, executing, and managing tasks.
    </action>
    <action>
**[Section 4: Testing Strategy](./sections/4-testing-strategy.md)**
- **Content:** Testing principles, scoping, documentation, and failure protocols.
- **Usage:** Designing test plans and implementing tests.
    </action>
    <action>
**[Section 5: Quick Reference](./sections/5-quick-reference.md)**
- **Content:** Common patterns, checklists, decision trees, and best practices.
- **Usage:** Quick day-to-day guidance and troubleshooting.
    </action>
</step>

<step n="3" goal="Follow the AI Agent's Pre-Work Checklist">
    <action>Before starting any work, follow this decision guide:</action>
    <action>
1. **Check for an associated PBI.**
   - If NO, create a PBI first.
   - If YES, proceed.
    </action>
    <action>
2. **Verify PBI plan is approved.**
   - If NO, create a plan and get User approval.
   - If YES, proceed.
    </action>
    <action>
3. **Ensure tasks are created and auto-approved.**
   - If NO, decompose the plan into tasks (auto-set to "Agreed").
   - If YES, proceed.
    </action>
    <action>
4. **Confirm task status is "Agreed" in the task file.**
   - If NO, STOP and report the issue.
   - If YES, proceed.
    </action>
    <action>
5. **Check for other "InProgress" tasks for this PBI.**
   - If YES, STOP. Only one task can be "InProgress" at a time.
   - If NO, you may proceed with the work.
    </action>
</step>

<step n="4" goal="Adhere to Implementation Guidelines">
    <action>During implementation, follow these rules:</action>
    <action>1. Make ONLY the changes defined within the task's scope.</action>
    <action>2. Reference the task ID in ALL version control commits.</action>
    <action>3. Update all relevant documentation as you implement changes.</action>
    <action>4. Run tests continuously to ensure no regressions.</action>
    <action>5. If scope needs to expand, STOP and create a new task.</action>
</step>

<step n="5" goal="Complete the Pre-Submission Checklist">
    <action>Before submitting work for review, verify the following:</action>
    <action>[ ] All task requirements have been implemented.</action>
    <action>[ ] All tests are passing.</action>
    <action>[ ] A list of modified files is documented.</action>
    <action>[ ] The task status has been updated in its respective file.</action>
    <action>[ ] The task history has been logged.</action>
    <action>[ ] The task file has been committed (the index will auto-generate).</action>
</step>

<step n="6" goal="Review Critical Rules and Stop Conditions">
    <action>Memorize and adhere to these critical rules.</action>
    <action>
**Absolutely Mandatory:**
1. NO code changes without an agreed task.
2. NO task creation without an associated PBI.
3. ALWAYS update task status in the task file.
4. ONLY ONE task "InProgress" per PBI at a time.
5. ALL status changes must be logged in history.
6. NO file creation without User confirmation.
7. NO scope creep; stay within task boundaries.
8. EVALUATE remaining tasks for obsolescence after each completion.
    </action>
    <action>
**When to STOP and Report to User:**
- Multiple "InProgress" tasks found for the same PBI.
- Task is not in "Agreed" state.
- Scope is expanding beyond the task definition.
- Tests are failing (follow Test Failure Protocol from Section 4).
- Test expectation mismatch (escalate immediately).
- Dependencies are unavailable (mark task as "Blocked").
    </action>
</step>

<step n="7" goal="Navigate the Documentation">
    <action>Use the following links for quick navigation:</action>
    <action>- [Section 1: Fundamentals](./sections/1-fundamentals.md)</action>
    <action>- [Section 2: PBI Management](./sections/2-pbi-management.md)</action>
    <action>- [Section 3: Task Management](./sections/3-task-management.md)</action>
    <action>- [Section 4: Testing Strategy](./sections/4-testing-strategy.md)</action>
    <action>- [Section 5: Quick Reference](./sections/5-quick-reference.md)</action>
</step>

<step n="8" goal="Review Document History">
    <action>Understand the evolution of this policy.</action>
    <action>
| Version | Date       | Changes                                                                                                     |
|---------|------------|-------------------------------------------------------------------------------------------------------------|
| 2.1     | 2025-10-20 | Added Single Source of Truth for task status, Test Failure Protocol, and Task Obsolescence Criteria.          |
| 2.0     | 2025-10-19 | Split into a modular structure for easier navigation.                                                         |
| 1.0     | 2025-10-19 | Initial monolithic version.                                                                                   |
    </action>
</step>

</workflow>
