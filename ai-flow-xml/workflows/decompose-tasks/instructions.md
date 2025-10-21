# Decompose Tasks - Workflow Instructions

<critical>The workflow execution engine is governed by: {project-root}/ai-flow-config.yaml</critical>
<critical>You MUST have already loaded and processed: {project-root}/workflows/decompose-tasks/workflow.yaml</critical>
<critical>Follow Task Management rules from: {project-root}/docs/rules/sections/3-task-management.md</critical>
<critical>Communicate in {communication_language} throughout task decomposition</critical>

<workflow>

<step n="0" goal="Load context and validate workflow.yaml">
<action>Verify that workflow.yaml has been loaded and processed</action>
<action>Extract standard config variables: {user_name}, {communication_language}, {output_folder}, {date}</action>
<check>IF workflow.yaml not loaded THEN HALT with error message</check>
<action>Greet {user_name} in {communication_language}</action>
</step>

<step n="1" goal="Collect required variables from user">
<ask>What is the PBI ID you want to decompose into tasks?

Example: 14

→ PBI ID:</ask>

<action>Store user response as {pbi_id}</action>
<action>Validate that PBI ID is numeric</action>

<template-output>user_variables</template-output>
</step>

<step n="2" goal="Load required context files and validate PBI state">
<action>Load the following files in order:</action>

<action>1. Primary directive: {project-root}/AGENTS.md</action>
<action>2. Configuration: {project-root}/ai-flow-config.yaml</action>
<action>3. Task management rules: {project-root}/docs/rules/sections/3-task-management.md</action>
<action>4. Task template structure: {installed_path}/TASK_TEMPLATE.md</action>
<critical>TASK_TEMPLATE.md defines the EXACT format you MUST use when creating task files</critical>
<critical>Study the template structure, YAML frontmatter, sections, and field descriptions</critical>
<action>5. Backlog: {output_folder}/backlog.md</action>
<action>6. PRD file: {output_folder}/{pbi_id}/prd.md</action>

<action>Parse backlog.md to verify:
- PBI {pbi_id} exists
- Extract current PBI status
</action>

<check>IF PBI does not exist THEN
  Display error in {communication_language}:
  "❌ ERROR: PBI {pbi_id} not found in backlog

  Please verify the PBI ID and try again."

  HALT workflow execution
END IF</check>

<check>IF PBI status != "ReadyForTasks" THEN
  Display error in {communication_language}:
  "❌ ERROR: PBI must be in 'ReadyForTasks' state

  Current state: {current_state}

  This PBI needs to be in 'ReadyForTasks' state before decomposition.

  Valid states for decomposition: ReadyForTasks
  Current state: {current_state}

  Please transition the PBI to ReadyForTasks first."

  HALT workflow execution
END IF</check>

<action>Parse PRD file to extract:
- PBI title
- Implementation Plan section (all phases)
- Requirements section
- Testing Strategy section
</action>

<template-output>loaded_context</template-output>
</step>

<step n="3" goal="Display context summary to user">
<action>Show context summary in {communication_language}:</action>

<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 CONTEXT LOADED & READY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**PBI ID:** {pbi_id}
**PBI Title:** {pbi_title}
**Current State:** ReadyForTasks ✓

**Implementation Plan Phases:** {phase_count}
Phase 1: {phase_1_name}
Phase 2: {phase_2_name}
...

**Requirements:** {requirement_count}

**Testing Strategy:** Unit, Integration, E2E

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Ready to decompose into tasks.
</example>

<template-output>context_summary</template-output>
</step>

<step n="4" goal="Analyze Implementation Plan and create task breakdown">
<action>Analyze Implementation Plan to identify atomic tasks</action>

<action>For each phase in Implementation Plan:
1. Identify distinct deliverables
2. Break down into atomic tasks (each task should be completable independently)
3. Each task should have clear goal, requirements, and verification
4. Group related changes into single tasks where appropriate
5. Separate frontend and backend tasks when applicable
6. Create dedicated testing tasks when needed
</action>

<action>Generate task list with the following for each task:
- Task ID: {pbi_id}-{sequential_number}
- Task Name: Clear, concise title (e.g., "Implement user authentication API")
- Goal: What this task accomplishes
- Requirements: Specific, measurable requirements
- Implementation steps: Detailed steps to complete
- Files to modify: List of files this task will change
- Verification: How to verify completion
- Testing: Test requirements for this task
</action>

<action>Display proposed task breakdown in {communication_language}:</action>
<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 PROPOSED TASK BREAKDOWN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Total Tasks: {task_count}

**Task {pbi_id}-1: {task_name_1}**
Goal: {goal_1}
Requirements: {req_count_1} items
Files: {file_count_1} to modify

**Task {pbi_id}-2: {task_name_2}**
Goal: {goal_2}
Requirements: {req_count_2} items
Files: {file_count_2} to modify

...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

This breakdown follows the Implementation Plan phases.
All tasks will be auto-approved (status: Agreed) since you already approved the plan.

Continue with task creation? (y/n)
</example>

<template-output>task_breakdown</template-output>
</step>

<step n="5" goal="Create task directory and individual task files">
<action>Create directory if not exists: {output_folder}/{pbi_id}/</action>

<action>For each task in breakdown:</action>

<action>Generate ISO 8601 timestamp using {date}</action>

<action>Create task file: {output_folder}/{pbi_id}/{pbi_id}-{n}.md</action>

<critical>Follow EXACTLY the structure from TASK_TEMPLATE.md loaded in Step 2</critical>
<critical>Include YAML frontmatter, all required sections, and proper formatting</critical>

<action>Write task content following TASK_TEMPLATE.md structure:</action>

<example>
---
priority: {priority}
created: {date}
updated: {date}
estimated_hours: {estimated_hours or "TBD"}
---

# Task: {task_id} {task_name}

[Back to task list](tasks.md)

## Goal

{clear_objective_statement}

## Context

**Parent PBI:** [{pbi_id} - {pbi_title}](prd.md)

{background_from_pbi}

This task is part of: {implementation_phase_name}

## Status History

| Timestamp | Action | From Status | To Status | Details | User |
|-----------|---------|-------------|-----------|---------|------|
| {timestamp} | Created | - | Proposed | Task created from PBI implementation plan | {user_name} |
| {timestamp} | State Transition | Proposed | Agreed | Auto-approved (plan already approved by user) | ai-agent |

## Requirements

- [ ] {requirement_1}
- [ ] {requirement_2}
- [ ] {requirement_3}
...

## Implementation Steps

1. Step 1: {step_name_1}
   - {action_1_1}
   - {action_1_2}

2. Step 2: {step_name_2}
   - {action_2_1}
   - {action_2_2}

...

## Files to Modify/Create

- [ ] {file_path_1} - {what_changes_1}
- [ ] {file_path_2} - {what_changes_2}
...

## Testing

**Test Cases:**
- [ ] {test_case_1}
- [ ] {test_case_2}
- [ ] {test_case_3}

**Coverage Target:** {coverage_percentage}%

## Success Criteria

- [ ] Implementation complete
- [ ] All requirements met
- [ ] Tests passing
- [ ] Code follows standards
- [ ] Documentation updated

## References

**Related Tasks:**
- Depends on: {dependent_task_ids or "None"}
- Blocks: {blocked_task_ids or "None"}

**Additional Notes:**
{any_additional_notes}
</example>

<action>Write task file to disk</action>

<action>Display progress in {communication_language}:</action>
<example>
✅ Created Task {task_id}: {task_name}
   Status: Agreed (auto-approved)
   Requirements: {req_count}
   Files: {file_count}
</example>

<template-output>task_files_created</template-output>
</step>

<step n="6" goal="Create task index file (tasks.md)">
<action>Generate task index file: {output_folder}/{pbi_id}/tasks.md</action>

<action>Create markdown table with all tasks:</action>

<example>
# Tasks for PBI-{pbi_id}: {pbi_title}

This file lists all tasks for this PBI.

**Parent PBI:** [PBI-{pbi_id}](./prd.md)

## Task Index

| Task ID | Name | Status | Description |
|---------|------|--------|-------------|
| [{pbi_id}-1](./{pbi_id}-1.md) | {task_name_1} | Agreed | {brief_description_1} |
| [{pbi_id}-2](./{pbi_id}-2.md) | {task_name_2} | Agreed | {brief_description_2} |
| [{pbi_id}-3](./{pbi_id}-3.md) | {task_name_3} | Agreed | {brief_description_3} |
...

## Summary

- **Total Tasks:** {task_count}
- **Status Distribution:**
  - Agreed: {agreed_count}
  - InProgress: 0
  - InReview: 0
  - Done: 0

## Notes

All tasks have been auto-approved since the parent PBI plan was already reviewed and approved.

To execute a task, use the `*execute-task` workflow.

---

**Last Updated:** {timestamp}
</example>

<action>Write tasks.md to disk</action>

<action>Confirm creation in {communication_language}:</action>
<example>
✅ Task Index Created

- File: {output_folder}/{pbi_id}/tasks.md
- Total tasks: {task_count}
- All tasks status: Agreed ✓
</example>

<template-output>task_index_created</template-output>
</step>

<step n="7" goal="Update PBI status to InProgress">
<action>Generate ISO 8601 timestamp using {date}</action>

<action>Update backlog.md:
- Change PBI {pbi_id} status from "ReadyForTasks" to "InProgress"
- Add history entry: | {timestamp} | {pbi_id} | State Transition | ReadyForTasks → InProgress - Tasks created and ready for implementation | {user_name} |
</action>

<action>Update PRD frontmatter in {output_folder}/{pbi_id}/prd.md:
- Set status: InProgress
- Set updated: {timestamp}
</action>

<action>Confirm transition in {communication_language}:</action>
<example>
✅ PBI Transitioned to InProgress

- Status: ReadyForTasks → InProgress
- History: Logged with timestamp
- PRD status: Updated
- Backlog: Updated

PBI is now in active implementation.
</example>

<template-output>transition_to_inprogress</template-output>
</step>

<step n="8" goal="Display final summary and next steps">
<action>Show comprehensive summary to {user_name} in {communication_language}:</action>

<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TASK DECOMPOSITION COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**PBI ID:** {pbi_id}
**PBI Title:** {pbi_title}
**New Status:** InProgress

**Tasks Created:** {task_count}

| Task ID | Name | Status |
|---------|------|--------|
| {pbi_id}-1 | {task_name_1} | Agreed |
| {pbi_id}-2 | {task_name_2} | Agreed |
| {pbi_id}-3 | {task_name_3} | Agreed |
...

**Files Created:**
- ✅ {output_folder}/{pbi_id}/tasks.md (task index)
- ✅ {output_folder}/{pbi_id}/{pbi_id}-1.md
- ✅ {output_folder}/{pbi_id}/{pbi_id}-2.md
- ✅ {output_folder}/{pbi_id}/{pbi_id}-3.md
...

**State Transitions:**
1. ReadyForTasks → InProgress ({timestamp})

**All tasks auto-approved:**
Since you already approved the PRD implementation plan, all tasks are automatically set to "Agreed" status and ready for execution.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📋 Next Steps:**

1. **Review Task Files** (optional)
   - Open: {output_folder}/{pbi_id}/tasks.md
   - Review individual task files
   - Verify breakdown makes sense

2. **Execute Tasks**
   - Start with Task {pbi_id}-1
   - Use `*execute-task` workflow
   - Provide task path: {output_folder}/{pbi_id}/{pbi_id}-1.md

3. **Task Execution Rules**
   - Only 1 task can be InProgress at a time
   - Complete tasks in logical order
   - Each task transitions: Agreed → InProgress → InReview → Done

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📂 Quick Links:**

- Task Index: [{output_folder}/{pbi_id}/tasks.md]({output_folder}/{pbi_id}/tasks.md)
- PRD: [{output_folder}/{pbi_id}/prd.md]({output_folder}/{pbi_id}/prd.md)
- Backlog: [{output_folder}/backlog.md]({output_folder}/backlog.md)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Workflow execution completed successfully! 🎉**

You can now start executing tasks using the `*execute-task` workflow.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<template-output>final_summary</template-output>
</step>

</workflow>
