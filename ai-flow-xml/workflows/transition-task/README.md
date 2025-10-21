# Transition Task Workflow

Manually change task state without full implementation workflow.

## Purpose

This workflow allows you to manually transition a task from one state to another without going through the full `*execute-task` workflow. Useful for:

- **Correcting states** - Fix incorrect task states
- **Manual updates** - Mark tasks as blocked, cancelled, or done manually
- **Skipping implementation** - Move tasks forward without AI implementation
- **State management** - Explicit state control when needed

## When to Use

### Common Scenarios

1. **Mark as Blocked** - External blocker prevents progress
2. **Cancel Task** - Task no longer needed
3. **Mark as Done** - Completed manually outside AI workflow
4. **Reset to Agreed** - Restart task from beginning
5. **Move to Testing** - Skip straight to testing phase

### When NOT to Use

- **Full implementation** - Use `*execute-task` instead
- **Creating tasks** - Use `*decompose-tasks` instead
- **Invalid transitions** - Must follow allowed transitions

## Workflow Type

- **Interactive:** Yes - requires confirmation
- **Duration:** 2-5 minutes
- **Complexity:** Simple

## Prerequisites

- Task file must exist
- Current state must allow transition to target state
- Concurrency rules must be respected (for InProgress)

## Inputs

### Required

- **task_path** - Full path to task file (e.g., "docs/delivery/14/14-1.md")
- **target_state** - Desired state (Proposed, Agreed, InProgress, InReview, Testing, Blocked, Done, Cancelled)

### Optional

- **transition_reason** - Why this transition is needed (recommended for audit trail)

## Process Overview

```
1. Collect task path and target state
2. Load task and extract current state
3. Validate transition is allowed
4. Check concurrency rules (if transitioning to InProgress)
5. Confirm with user
6. Update Status History
7. Update task index
8. Update backlog (if enabled)
```

## Valid Transitions

Defined in `ai-flow-config.yaml` → `task.transitions`:

| From | To | Use Case |
|------|-----|----------|
| Proposed | Agreed | User approves task |
| Proposed | Cancelled | Task not needed |
| Agreed | InProgress | Start work manually |
| Agreed | Cancelled | Task obsolete |
| InProgress | InReview | Implementation done |
| InProgress | Blocked | Cannot proceed |
| InReview | Testing | Ready for tests |
| InReview | InProgress | Needs rework |
| Testing | Done | Tests passed |
| Testing | InProgress | Tests failed |
| Blocked | InProgress | Blocker resolved |
| Blocked | Cancelled | Cannot resolve |

## Outputs

### Files Modified

- `{task_path}` - Status History table updated
- `docs/delivery/{pbi_id}/tasks.md` - Status column updated
- `docs/delivery/backlog.md` - Task status (if auto-update enabled)

### State Changes

Current state → Target state with:
- Timestamp (ISO 8601)
- Transition reason
- User who made change
- Logged in Status History

## Usage

### Command
```
*transition-task
```

### Example Session

```
AI: What is the path to the task?
User: docs/delivery/14/14-3.md

AI: What state do you want to transition to?
User: Blocked

AI: Why is this transition needed?
User: Waiting for API key from external service

AI: [Displays validation]
    ✅ Transition validation complete
    - Current: InProgress
    - Target: Blocked
    - Transition: Valid ✓
    - Concurrency: OK ✓

    Proceed with transition? (y/n)

User: y

AI: ✅ Task State Transition Complete
    State: InProgress → Blocked
    Reason: Waiting for API key from external service
```

## Validation Checks

Before transitioning, the workflow validates:

1. **State Validity** - Target state exists in config
2. **Transition Validity** - Transition is allowed
3. **Concurrency Rules** - Max 1 task InProgress (if applicable)
4. **File Existence** - Task file exists and is readable
5. **User Confirmation** - User approves the transition

## Critical Rules

### Concurrency Check

When transitioning to `InProgress`:
- Checks if another task is already InProgress
- Enforces `max_concurrent: 1` rule
- **HALTS** if limit reached
- User must complete/cancel other tasks first

### History Logging

ALL transitions logged with:
- ISO 8601 timestamp
- Event type: "State Transition"
- From state
- To state
- Reason/details
- User who made change

### Valid Transitions Only

Cannot skip states or make invalid transitions:
- ❌ Cannot: Agreed → Done (skips implementation)
- ❌ Cannot: InProgress → Cancelled (must resolve first)
- ✅ Can: InProgress → Blocked (valid path)
- ✅ Can: Blocked → Cancelled (valid path)

## Common Use Cases

### 1. Mark Task as Blocked

```
Scenario: Waiting for external dependency
Current: InProgress
Target: Blocked
Reason: "Waiting for third-party API access"
```

### 2. Cancel Obsolete Task

```
Scenario: Task no longer needed
Current: Agreed
Target: Cancelled
Reason: "Requirement removed from PBI"
```

### 3. Manually Mark as Done

```
Scenario: Completed outside AI workflow
Current: InReview
Target: Done
Reason: "Manually tested and verified - all criteria met"
```

### 4. Unblock and Resume

```
Scenario: Blocker resolved
Current: Blocked
Target: InProgress
Reason: "API key received, can continue implementation"
```

### 5. Send Back for Rework

```
Scenario: Review found issues
Current: InReview
Target: InProgress
Reason: "Edge case not handled - needs additional logic"
```

## Success Criteria

✅ Transition is valid per config
✅ Concurrency rules respected
✅ Status History updated
✅ Task index updated
✅ User confirmed transition
✅ History logged with timestamp
✅ Files updated correctly

## Error Handling

### Invalid Target State
Shows valid states from config

### Invalid Transition
Shows allowed transitions from current state

### Concurrency Limit Reached
Lists active tasks, user must resolve

### File Not Found
Prompts to verify path

## Next Steps After Transition

### After → InProgress
- Continue implementation
- Mark as InReview when done

### After → Blocked
- Document blocker details
- Work on other tasks
- Transition back when resolved

### After → Cancelled
- Update PBI if needed
- Check if other tasks need adjustment

### After → Done
- Check if all PBI tasks complete
- Consider transitioning PBI to InReview

## Configuration

Uses standard config from `ai-flow-config.yaml`:
- `user_name`
- `communication_language`
- `output_folder`
- `task.states`
- `task.transitions`
- `task.max_concurrent`

## Policy Compliance

Follows **Section 3: Task Management**:
- Valid state transitions only
- Concurrency limits enforced
- History logging mandatory
- Task index kept synchronized

## Tips

1. **Always provide reason** - Helps with audit trail
2. **Check dependencies** - Consider impact on other tasks
3. **Update task notes** - Add context if needed
4. **Verify concurrency** - Don't exceed max InProgress
5. **Communicate with team** - If working collaboratively

## Comparison with execute-task

| Feature | transition-task | execute-task |
|---------|----------------|--------------|
| Changes state | ✅ Yes | ✅ Yes |
| Implements code | ❌ No | ✅ Yes |
| Runs tests | ❌ No | ✅ Yes |
| AI guidance | ❌ Minimal | ✅ Full |
| Duration | 2-5 min | 30-120 min |
| Use case | Manual state mgmt | Full implementation |

## Related Documentation

- [Task Management Policy](../../docs/rules/sections/3-task-management.md)
- [Execute Task Workflow](../execute-task/README.md)
- [Task States Reference](../../ai-flow-config.yaml)

---

**Version:** 2.0
**Last Updated:** 2025-10-21
