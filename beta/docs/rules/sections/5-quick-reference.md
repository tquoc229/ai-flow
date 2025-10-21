# Section 5: Quick Reference - Instructions

<critical>This document provides a quick reference for common patterns, commands, and checklists. Adherence is mandatory to ensure process consistency.</critical>
<critical>This is Section 5 of the AI Coding Agent Project Policy.</critical>

<workflow>

<step n="1" goal="Review Pattern 1: Starting a New PBI">
    <action>
        Follow this step-by-step process to start a new PBI:
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
    </action>
</step>

<step n="2" goal="Review Pattern 2: Working on a Task">
    <action>
        Follow this complete workflow when working on a task:
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
    </action>
    <action>
        Internalize these key points:
        - ✅ ALWAYS update both task file AND index together
        - ✅ NEVER have two tasks InProgress for same PBI
        - ✅ ALWAYS log status changes with timestamp
        - ✅ ALWAYS reference task ID in commits
    </action>
</step>

<step n="3" goal="Review Pattern 3: Handling Blocked Tasks">
    <action>
        Follow this complete workflow for handling blocked tasks:
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
    </action>
    <action>
        Recognize common blocking reasons:
        - 🔴 Waiting for external API credentials
        - 🔴 User needs to make architectural decision
        - 🔴 Dependency on another team's work
        - 🔴 Required third-party service unavailable
        - 🔴 Missing design specifications
        - 🔴 Technical limitation discovered
    </action>
    <action>
        Follow these rules while a task is blocked:
        - ✅ Document everything about the blocker
        - ✅ Propose solutions if possible
        - ✅ Work on other non-blocked tasks (different PBI)
        - ❌ Do NOT try to work around the blocker
        - ❌ Do NOT start other tasks in same PBI
    </action>
</step>

<step n="4" goal="Review Pattern 4: Creating Task Documentation">
    <action>
        Use this template for all task documentation files:
        ```markdown
        ---
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
    </action>
</step>

<step n="5" goal="Review Pattern 5: Status Synchronization Check">
    <action>
        Execute this validation logic before ANY status change:
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
    </action>
</step>

<step n="6" goal="Review Pattern 6: External Package Integration">
    <action>
        Follow this process when integrating a new external package:
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
    </action>
</step>

<step n="7" goal="Review Common Commands and Patterns">
    <action>
        Use this Git workflow for task completion:
        ```bash
        # When task moves to Done status, use the format from config.yaml:
        # {automation.commit_message_format}

        # Option 1: Using automation helper
        git acp "feat(1): 1-7 Add pino logging"

        # Option 2: Manual steps
        git add .
        git commit -m "feat(1): 1-7 Add pino logging"
        git push origin feature/1-7-add-pino-logging
        ```
    </action>
    <action>
        Use these commands to create a new PBI structure:
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
    </action>
    <action>
        Use these commands to check for task status mismatches:
        ```bash
        # Check for status mismatches

        # 1. Read task file status
        grep "Status" docs/delivery/<PBI-ID>/<PBI-ID>-<TASK-ID>.md

        # 2. Read index status
        grep "<TASK-ID>" docs/delivery/<PBI-ID>/tasks.md

        # 3. Compare - they must match!
        ```
    </action>
</step>

<step n="8" goal="Review Decision Tree: Should I Search for Package Documentation?">
    <action>
        Follow this decision tree before using an external package:
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
    </action>
</step>

<step n="9" goal="Review Decision Tree: What Test Plan Detail Level?">
    <action>
        Follow this decision tree to determine the required level of detail for a test plan:
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
    </action>
</step>

<step n="10" goal="Review Decision Tree: Can I Start This Task?">
    <action>
        Follow this step-by-step decision process before starting any task:
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
    </action>
    <action>
        Use this quick checklist before starting work:
        - [ ] Task status is "Agreed" ✅
        - [ ] Status matches in task file and index ✅
        - [ ] No other tasks "InProgress" for this PBI ✅
        - [ ] All dependencies available ✅

        If ALL checked → Start work!
    </action>
</step>

<step n="11" goal="Review Validation Checklists">
    <action>
        Use this checklist before starting ANY task:
        - [ ] Task exists in task file (`<PBI-ID>-<TASK-ID>.md`)
        - [ ] Task exists in task index (`tasks.md`)
        - [ ] Status is `Agreed` in task file
        - [ ] Status is `Agreed` in task index
        - [ ] No other tasks `InProgress` for this PBI
        - [ ] All dependencies are available
        - [ ] Implementation plan is clear
        - [ ] Test plan is defined
    </action>
    <action>
        Use this checklist before submitting a task for review:
        - [ ] All requirements implemented
        - [ ] All tests written and passing
        - [ ] Test plan requirements met
        - [ ] Code follows project standards
        - [ ] Files modified documented in task file
        - [ ] Task documentation updated with implementation details
        - [ ] No scope creep (all changes within task scope)
        - [ ] Commits reference task ID
    </action>
    <action>
        Use this checklist before marking a task as Done:
        - [ ] User has reviewed and approved
        - [ ] All acceptance criteria verified
        - [ ] All tests passing
        - [ ] Status updated in BOTH locations
        - [ ] Next tasks reviewed for relevance
        - [ ] User confirmed about next tasks
        - [ ] Version control workflow executed
        - [ ] Completion logged in history
    </action>
    <action>
        Use this checklist before marking a PBI as Done:
        - [ ] ALL tasks have status `Done`
        - [ ] All Conditions of Satisfaction met
        - [ ] Full test suite passing
        - [ ] Documentation updated
        - [ ] E2E CoS test completed and passing
        - [ ] User approval received
        - [ ] Completion date recorded
        - [ ] History logged
    </action>
</step>

<step n="12" goal="Review Common Pitfalls to Avoid">
    <action>
        Avoid these File Management pitfalls:
        - ❌ **Creating files without User confirmation**: Always get explicit approval for new standalone files.
        - ❌ **Orphaned task files**: Every task in the index MUST have a corresponding markdown file.
        - ❌ **Broken links**: Use relative paths and test all links.
    </action>
    <action>
        Avoid these Status Management pitfalls:
        - ❌ **Updating only one location**: ALWAYS update both the task file AND index atomically.
        - ❌ **Skipping status history**: ALWAYS log status changes with a timestamp.
        - ❌ **Invalid transitions**: Follow the defined workflow states.
    </action>
    <action>
        Avoid these Workflow pitfalls:
        - ❌ **Multiple InProgress tasks**: Only ONE task per PBI should be InProgress.
        - ❌ **Scope creep**: Stay within the defined task scope. Create new tasks for additional work.
        - ❌ **Missing User approval**: Never start work on `Proposed` tasks.
    </action>
    <action>
        Avoid these Testing pitfalls:
        - ❌ **Over-engineering simple test plans**: Match test plan detail to task complexity.
        - ❌ **Skipping E2E CoS test task**: Every PBI MUST have a dedicated E2E testing task.
        - ❌ **Testing package APIs directly**: Do not test external package methods in your unit tests.
    </action>
    <action>
        Avoid these Documentation pitfalls:
        - ❌ **Duplicating information**: Follow the DRY principle; reference instead of duplicating.
        - ❌ **Using magic numbers**: Define constants for repeated or special values.
        - ❌ **Missing package guides**: Always create a guide when using a new package.
    </action>
</step>

<step n="13" goal="Review Best Practices Summary">
    <action>
        Adhere to these Process Best Practices:
        1. **Always verify before acting**: Check status, validate transitions, confirm no conflicts.
        2. **Keep tasks small and focused**: Break down complex features into atomic units.
        3. **Document as you go**: Keep documentation current in real-time.
        4. **Synchronize atomically**: Update related files in the same commit.
    </action>
    <action>
        Adhere to these Code Best Practices:
        1. **Use constants for values**: Define once, reference everywhere.
        2. **Follow DRY principle**: Maintain a single source of truth.
        3. **Reference task ID in commits**: Ensure every commit is traceable to a task.
        4. **Stay within scope**: Only make changes authorized by the task.
    </action>
    <action>
        Adhere to these Testing Best Practices:
        1. **Match test plan to complexity**: Don't over-engineer simple test plans.
        2. **Use real infrastructure for integration**: Mock only external third-party services.
        3. **Focus E2E on critical paths**: Reserve E2E tests for important user workflows.
        4. **Automate all tests**: Ensure consistent verification and fast feedback.
    </action>
    <action>
        Adhere to these Communication Best Practices:
        1. **Ask when uncertain**: Never assume user intent; clarify ambiguous requirements.
        2. **Report issues immediately**: Don't hide problems; stop work when blocked.
        3. **Review next tasks**: Confirm relevance with the user before finishing the current task.
        4. **Keep stakeholders informed**: Notify on status changes, blockers, and completions.
    </action>
</step>

<step n="14" goal="Navigate Between Sections">
    <action>Use the following links to navigate the policy documents:</action>
    - [← Back to Index](../project-policy-index.md)
    - [← Previous: Section 4 - Testing Strategy](./4-testing-strategy.md)
</step>

</workflow>