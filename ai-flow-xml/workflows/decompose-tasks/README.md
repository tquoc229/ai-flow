# Decompose Tasks Workflow

Break down an approved PBI plan into concrete, implementable tasks.

## Purpose

This workflow guides AI agents through:
- Reading approved PRD Implementation Plan
- Creating atomic task definitions
- Generating task.md index
- Creating individual task detail files
- Auto-approving all tasks (status: Agreed)
- Transitioning PBI: ReadyForTasks → InProgress

## When to Use

Use this workflow when:
- PBI status = **ReadyForTasks** (plan approved by user)
- Ready to break down plan into executable tasks
- Before starting implementation

## Files in This Workflow

- **workflow.yaml** - Configuration, variables, and metadata
- **instructions.md** - Step-by-step execution guide (XML structure)
- **checklist.md** - Validation checklist for task decomposition
- **TASK_TEMPLATE.md** - Standard template for individual task files
- **README.md** - This documentation

## How to Invoke

### From AGENTS.md Menu
```
*decompose-tasks
```

### Direct Load
```
Load: workflows/decompose-tasks/workflow.yaml
```

## Prerequisites

- PBI exists with status = "ReadyForTasks"
- PRD file complete and approved
- User has reviewed and approved the plan

## Key Features

- **Auto-Approval**: All tasks set to "Agreed" (plan already approved)
- **Atomic Tasks**: Each task is independently implementable
- **Complete Structure**: All required sections in task files
- **Index Generation**: tasks.md created automatically

## Inputs Required

1. **pbi_id** (required)
   - Example: "14"
   - PBI must exist with status "ReadyForTasks"

## Outputs Generated

1. **Task Index** (`{pbi_id}/tasks.md`)
   - Markdown table with all tasks
   - Columns: Task ID, Name (linked), Status, Description

2. **Task Detail Files** (`{pbi_id}/{pbi_id}-{n}.md`)
   - One file per task
   - Complete structure (Goal, Context, Requirements, Implementation Steps, etc.)
   - Status: Agreed
   - History: Created + Auto-Approved

3. **Backlog Update** (`backlog.md`)
   - PBI status: ReadyForTasks → InProgress
   - History entry logged

## Success Criteria

- ✅ All Implementation Plan phases → tasks
- ✅ Each task atomic and clear
- ✅ All tasks have complete detail files
- ✅ All tasks status = "Agreed"
- ✅ tasks.md index created
- ✅ PBI status = "InProgress"

## Task File Structure

Each task file follows a standard template (see [TASK_TEMPLATE.md](TASK_TEMPLATE.md)):

### YAML Frontmatter
```yaml
---
priority: medium
created: YYYY-MM-DD
updated: YYYY-MM-DD
estimated_hours: X
---
```

### Required Sections
1. **Title**: `# Task: {pbi_id}-{n} {task_name}`
2. **Goal**: Clear objective statement
3. **Context**: Why this task, background from PBI
4. **Status History**: Table tracking all state transitions
5. **Requirements**: Checkbox list of specific requirements
6. **Implementation Steps**: Numbered, detailed steps
7. **Files to Modify/Create**: Checkbox list with descriptions
8. **Testing**: Test cases and coverage target
9. **Success Criteria**: Completion checklist
10. **References**: Related tasks and notes

For detailed template documentation, see [TASK_TEMPLATE.md](TASK_TEMPLATE.md).

## Related Workflows

- **create-pbi** - Previous step: create and plan PBI
- **execute-task** - Next step: implement each task

## Context Loaded

This workflow loads (~1000 lines):
- AGENTS.md - Primary directive
- ai-flow-config.yaml - Task structure rules
- Section 3: Task Management
- backlog.md - Verify PBI status
- {pbi_id}/prd.md - Implementation Plan to decompose

## Estimated Duration

10-30 minutes (depends on plan complexity)

## Decomposition Strategy

1. **Read Implementation Plan phases** from PRD
2. **Identify atomic units of work** - each can be done independently
3. **Create 1 task per atomic unit**
4. **Ensure logical order** - dependencies clear
5. **Balance granularity** - not too big, not too small (typically 2-8 hours per task)

## Version

2.0 (BMAD-inspired structure)
