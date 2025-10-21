# Execute Task Workflow

Execute a MyFlow task from **Agreed** to **InReview** state with full implementation, testing, and validation.

## Purpose

This workflow guides AI agents through:
- Validating preconditions (task state, concurrency limits)
- Implementing all requirements with AI-suggested approaches
- Writing and running tests
- Executing verification steps
- Documenting implementation
- Transitioning task states
- Creating git commits

## When to Use

Use this workflow when:
- Task is in **Agreed** state
- User is ready to implement the task
- No other tasks are InProgress (max_concurrent: 1)

## Files in This Workflow

- **workflow.yaml** - Configuration, variables, and metadata
- **instructions.md** - Step-by-step execution guide (XML structure)
- **checklist.md** - Validation checklist (100+ items)

## How to Invoke

### From AGENTS.md Menu
```
*execute-task
```

### Direct Load
```
Load: workflows/execute-task/workflow.yaml
```

## Prerequisites

- Task file exists and is readable
- Task status = "Agreed"
- No other tasks InProgress for same PBI
- All policy files accessible

## Key Features

- **AI Elicitation**: Suggests 3 implementation approaches per requirement
- **Strict Mode**: Halts on test failures (configurable)
- **Comprehensive Validation**: 100+ checklist items
- **State Management**: Automatic status transitions with history logging
- **Git Integration**: Optional commit creation

## Inputs Required

1. **task_path** (required)
   - Example: `docs/delivery/14/14-1.md`
   - Extracted: `{pbi_id}`, `{task_id}`

2. **run_tests** (optional, default: "auto")
   - Options: auto, manual, skip

3. **auto_commit** (optional, default: false)
   - Create git commit after completion

## Outputs Generated

1. **Task File** (`{task_path}`)
   - Status History updated (2 transitions)
   - Implementation Notes appended

2. **Task Index** (`{pbi_id}/tasks.md`)
   - Status: Agreed → InProgress → InReview

3. **Backlog** (`backlog.md`, if enabled)
   - Reflects task state changes

4. **Git Commit** (optional)
   - Format: `feat({pbi_id}): {task_title}`

## Success Criteria

- ✅ All requirements implemented
- ✅ All tests passing
- ✅ All verifications passed
- ✅ Documentation complete
- ✅ Task state = InReview
- ✅ No policy violations

## Related Workflows

- **create-pbi** - Create parent PBI
- **decompose-tasks** - Break down PBI into tasks
- **transition-task** - Simple state changes

## Context Loaded

This workflow loads (~1500 lines):
- AGENTS.md - Primary directive
- config.yaml - Configuration
- Section 3: Task Management
- Section 4: Testing Strategy
- Task file
- Parent PBI PRD
- Task index

## Estimated Duration

30-120 minutes (varies by task complexity)

## Version

2.0 (BMAD-inspired structure)
