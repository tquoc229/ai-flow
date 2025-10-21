# Approve PBI Plan Workflow

User reviews and approves or rejects a PBI plan (PRD).

## Purpose

This workflow facilitates the critical approval step where the user reviews the AI-generated PRD (Product Requirements Document) and decides whether to:
- **Approve** the plan and proceed to task creation
- **Request revisions** with specific feedback for AI to address

## When to Use

- After `*create-pbi` workflow generates a PRD
- When PBI is in `PlanInReview` state
- When revising a rejected plan (`NeedsPlanRework` state)

## Workflow Type

- **Interactive:** Yes - requires user decision
- **Duration:** 5-15 minutes
- **Complexity:** Simple

## Prerequisites

- PBI must exist in backlog
- PBI status should be `PlanInReview` or `NeedsPlanRework`
- PRD file must exist: `docs/delivery/{pbi_id}/prd.md`

## Inputs

### Required
- **pbi_id** - The PBI ID to review (e.g., "14")

### Optional
- **show_full_prd** - Display complete PRD instead of summary (default: false)

## Process Overview

```
1. Load PBI and PRD
2. Display PRD Summary
3. User Reviews
4. User Decides:
   ├─ Approve → ReadyForTasks
   ├─ Request Revisions → NeedsPlanRework (with feedback)
   └─ Cancel → No changes
5. Update status and log history
```

## Outputs

### Files Modified
- `docs/delivery/backlog.md` - PBI status updated
- `docs/delivery/{pbi_id}/prd.md` - Status and feedback (if rejected)

### State Transitions

**If Approved:**
```
PlanInReview → ReadyForTasks
NeedsPlanRework → ReadyForTasks
```

**If Rejected:**
```
PlanInReview → NeedsPlanRework
```

## Usage

### Command
```
*approve-pbi-plan
```

### Example Session

```
AI: Which PBI plan would you like to review?
User: 14

AI: [Displays PRD Summary]
    - User Story
    - Requirements (5 functional, 3 non-functional)
    - Technical Approach
    - Legacy Discovery Findings (3 components found)
    - Implementation Plan (4 phases)
    - Testing Strategy
    - Success Criteria (6 items)

AI: What would you like to do?
    1. Approve
    2. Request Revisions
    3. View Full PRD
    4. Cancel

User: 1

AI: ✅ PBI Plan Approved
    Status: PlanInReview → ReadyForTasks

    Next Steps:
    - Use *decompose-tasks to break down into tasks
```

## What Gets Reviewed

The workflow presents:

### PRD Summary
- **Context:** Why, Current Situation, Desired State
- **Requirements:** Functional and non-functional
- **Technical Approach:** Architecture, components, tech stack
- **Legacy Discovery:** Reusable code found
- **Implementation Plan:** Phases and steps
- **Testing Strategy:** Unit, integration, E2E tests
- **Success Criteria:** Measurable completion criteria

### User Can
- Review summary (quick)
- View full PRD (detailed)
- Approve plan
- Request specific revisions
- Cancel without changes

## Approval Path

When user approves:
1. PBI status → `ReadyForTasks`
2. History logged with approval
3. PRD frontmatter updated
4. Ready for `*decompose-tasks` workflow

## Revision Path

When user requests revisions:
1. User provides specific feedback
2. PBI status → `NeedsPlanRework`
3. Feedback appended to PRD
4. History logged with revision request
5. AI revises plan based on feedback
6. User reviews again using this workflow

## Common Reasons for Revision

- **Requirements unclear** - Need more specificity
- **Technical approach inappropriate** - Better architecture needed
- **Implementation plan lacks detail** - Need more concrete steps
- **Testing strategy insufficient** - Need better coverage
- **Legacy Discovery incomplete** - Missing reusable components
- **Success criteria ambiguous** - Need measurable criteria

## Success Criteria

✅ User reviewed PRD summary
✅ User made informed decision
✅ Status transitioned correctly
✅ History logged with timestamp
✅ Files updated appropriately
✅ (If rejected) Feedback captured clearly

## Next Workflows

### After Approval
- **decompose-tasks** - Break plan into executable tasks

### After Rejection
- AI revises PRD based on feedback
- User runs `*approve-pbi-plan` again to review revisions

## Configuration

Uses standard config from `ai-flow-config.yaml`:
- `user_name`
- `communication_language`
- `output_folder`
- `pbi.states`
- `pbi.transitions`

## Policy Compliance

Follows **Section 2: PBI Management**:
- Valid state transitions
- History logging
- Bidirectional links maintained
- Status source of truth (backlog.md)

## Tips

1. **Review carefully** - This determines task breakdown quality
2. **Be specific with feedback** - AI needs clear guidance for revisions
3. **Check Legacy Discovery** - Ensure code reuse is maximized
4. **Verify Testing Strategy** - Adequate coverage planned
5. **Validate Requirements** - All CoS addressed

## Related Documentation

- [PBI Management Policy](../../docs/rules/sections/2-pbi-management.md)
- [Create PBI Workflow](../create-pbi/README.md)
- [Decompose Tasks Workflow](../decompose-tasks/README.md)

---

**Version:** 2.0
**Last Updated:** 2025-10-21
