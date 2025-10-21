# Transition Task - Workflow Instructions

<critical>The workflow execution engine is governed by: {project-root}/ai-flow-config.yaml</critical>
<critical>You MUST have already loaded and processed: {project-root}/workflows/transition-task/workflow.yaml</critical>
<critical>Validate ALL transitions against config before executing</critical>
<critical>Communicate in {communication_language} throughout workflow</critical>

<workflow>

<step n="0" goal="Load context and validate workflow.yaml">
<action>Verify that workflow.yaml has been loaded and processed</action>
<action>Extract standard config variables: {user_name}, {communication_language}, {output_folder}, {date}</action>
<check>IF workflow.yaml not loaded THEN HALT with error message</check>
<action>Greet {user_name} in {communication_language}</action>
</step>

<step n="1" goal="Collect required variables from user">
<ask>What is the path to the task you want to transition?

Example: docs/delivery/14/14-1.md

→ Task path:</ask>

<action>Store user response as {task_path}</action>
<action>Extract {pbi_id} and {task_id} from path using pattern: delivery/{pbi_id}/{task_id}.md</action>

<ask>What state do you want to transition this task to?

Available states:
- Proposed
- Agreed
- InProgress
- InReview
- Testing
- Blocked
- Done
- Cancelled

→ Target state:</ask>

<action>Store user response as {target_state}</action>

<ask>Why is this manual transition needed? (Optional but recommended)

→ Reason:</ask>

<action>Store user response as {transition_reason} (default: "Manual state transition")</action>

<template-output>user_variables</template-output>
</step>

<step n="2" goal="Load required context and extract current state">
<action>Load the following files in order:</action>

<action>1. Configuration: {project-root}/ai-flow-config.yaml</action>
<action>2. Task management rules: {project-root}/docs/rules/sections/3-task-management.md</action>
<action>3. Task file: {task_path}</action>
<action>4. Task index: {output_folder}/{pbi_id}/tasks.md</action>

<check>IF task file does not exist THEN
  Display error in {communication_language}:
  "❌ ERROR: Task file not found

  Path: {task_path}

  Please verify the path and try again."

  HALT workflow execution
END IF</check>

<action>Parse task file and extract:
- Current status (from Status History table - last entry)
- Task title
- All requirements
</action>

<action>Parse ai-flow-config.yaml and extract:
- Valid task states (task.states)
- Valid transitions (task.transitions)
- Max concurrent tasks (task.max_concurrent)
</action>

<template-output>loaded_context</template-output>
</step>

<step n="3" goal="Validate target state and transition">
<action>Check 3.1: Target State Validity</action>
<check>IF {target_state} NOT IN task.states from config THEN
  Display error in {communication_language}:
  "❌ ERROR: Invalid target state

  Target state: {target_state}

  Valid states from config:
  {list all valid states}

  Please choose a valid state."

  HALT workflow execution
END IF</check>

<action>Check 3.2: Transition Validity</action>
<check>IF transition from {current_state} to {target_state} NOT IN task.transitions THEN
  Display error in {communication_language}:
  "❌ ERROR: Invalid state transition

  Current state: {current_state}
  Target state: {target_state}

  This transition is not allowed per ai-flow-config.yaml

  Valid transitions from {current_state}:
  {list allowed target states}

  Please choose a valid transition."

  HALT workflow execution
END IF</check>

<action>Check 3.3: Concurrency Rule (if target is InProgress)</action>
<check>IF {target_state} == "InProgress" THEN
  Parse task index: {output_folder}/{pbi_id}/tasks.md
  Count tasks with status = "InProgress"

  IF count >= {task.max_concurrent} THEN
    Display error in {communication_language}:
    "❌ ERROR: Concurrency limit reached

    Maximum concurrent tasks: {task.max_concurrent}
    Currently InProgress: {count}

    Active task(s):
    {list active tasks}

    You must complete or cancel active tasks before starting this one.

    HALT: Cannot proceed with transition to InProgress."

    HALT workflow execution
  END IF
END IF</check>

<action>Display validation summary in {communication_language}:</action>
<example>
✅ TRANSITION VALIDATION COMPLETE

- Current State: {current_state}
- Target State: {target_state}
- Transition: Valid ✓
- Concurrency: OK ✓ ({count}/{max_concurrent} active)
- All Policies: Compliant ✓

Transition is allowed and can proceed.
</example>

<template-output>validation_status</template-output>
</step>

<step n="4" goal="Confirm transition with user">
<action>Display transition summary and ask for confirmation in {communication_language}:</action>

<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔄 TASK STATE TRANSITION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Task:** {task_id} - {task_title}
**File:** {task_path}

**Transition:**
{current_state} → {target_state}

**Reason:** {transition_reason}

**Impact:**
- Status History: New entry will be added
- Task index: Status column will update
- Backlog: Will update (if auto-update enabled)

**Files to be modified:**
1. {task_path} (Status History table)
2. {output_folder}/{pbi_id}/tasks.md (Status column)
{IF auto_update_backlog}
3. {output_folder}/backlog.md (Task status)
{END IF}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Proceed with this transition? (y/n)
</example>

<ask>→ Your confirmation (y/n):</ask>

<action>Store user response as {confirmed}</action>

<check>IF {confirmed} != "y" and {confirmed} != "yes" THEN
  Display in {communication_language}:
  "ℹ️ Transition cancelled. No changes made.

  Task remains in {current_state} state."

  HALT workflow execution
END IF</check>

<template-output>user_confirmation</template-output>
</step>

<step n="5" goal="Execute state transition">
<action>Generate ISO 8601 timestamp using {date}</action>

<action>Create history entry:
| {timestamp} | State Transition | {current_state} | {target_state} | {transition_reason} | {user_name} |
</action>

<action>Update {task_path}:
- Find Status History table
- Append new row with history entry
- Preserve all existing content
</action>

<action>Update {output_folder}/{pbi_id}/tasks.md:
- Find row for task {task_id}
- Change Status column from {current_state} to {target_state}
- Preserve all other content
</action>

<action if="config: automation.auto_update_backlog == true">
  Update {output_folder}/backlog.md:
  - Find task entry (if exists)
  - Update task status to {target_state}
</action>

<action>Confirm transition in {communication_language}:</action>
<example>
✅ Task State Transition Complete

- State: {current_state} → {target_state}
- Timestamp: {timestamp}
- History: Logged in Status History table
- Task index: Updated
{IF auto_update_backlog}
- Backlog: Updated
{END IF}

Task is now in {target_state} state.
</example>

<template-output>transition_complete</template-output>
</step>

<step n="6" goal="Display final summary and next steps">
<action>Show comprehensive summary to {user_name} in {communication_language}:</action>

<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TASK TRANSITION COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Task:** {task_id} - {task_title}
**Previous State:** {current_state}
**New State:** {target_state}
**Timestamp:** {timestamp}
**Reason:** {transition_reason}

**Files Modified:**
- ✅ {task_path} (Status History updated)
- ✅ {output_folder}/{pbi_id}/tasks.md (Status column updated)
{IF auto_update_backlog}
- ✅ {output_folder}/backlog.md (Task status updated)
{END IF}

**Transition Logged:**
| {timestamp} | State Transition | {current_state} | {target_state} | {transition_reason} | {user_name} |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📋 Next Steps:**

{IF target_state == "Agreed"}
**Ready for Execution**
- Use `*execute-task` workflow to implement this task
- Provide task path: {task_path}

{ELSE IF target_state == "InProgress"}
**Task Now Active**
- Continue implementation
- Remember: max {task.max_concurrent} task(s) can be InProgress at a time
- Mark as InReview when implementation complete

{ELSE IF target_state == "InReview"}
**Ready for Review**
- Review the implementation
- Run tests if not done yet
- Transition to Testing or Done when satisfied

{ELSE IF target_state == "Testing"}
**In Testing Phase**
- Run acceptance tests
- Verify in staging environment
- Transition to Done when all tests pass

{ELSE IF target_state == "Blocked"}
**Task Blocked**
- Document the blocker in task notes
- Work on other tasks
- Transition back to InProgress when blocker resolved

{ELSE IF target_state == "Done"}
**Task Complete**
- Task is now closed
- Check if all PBI tasks are done
- If yes, transition PBI to InReview

{ELSE IF target_state == "Cancelled"}
**Task Cancelled**
- Task will not be worked on
- Update PBI if needed
- Consider if other tasks need adjustment

{ELSE}
**Task Updated**
- State: {target_state}
- Follow appropriate workflow for this state

{END IF}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📂 Quick Links:**

- Task file: [{task_id}.md]({task_path})
- Task index: [tasks.md]({output_folder}/{pbi_id}/tasks.md)
- PBI: [PBI {pbi_id}]({output_folder}/{pbi_id}/prd.md)
- Backlog: [backlog.md]({output_folder}/backlog.md)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Workflow execution completed successfully! 🎉**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<template-output>final_summary</template-output>
</step>

</workflow>
