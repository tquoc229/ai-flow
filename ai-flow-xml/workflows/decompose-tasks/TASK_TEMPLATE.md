# Task File Template

This is the standard template used by the `*decompose-tasks` workflow when creating individual task files.

## Template Structure

```markdown
---
priority: medium
created: YYYY-MM-DD
updated: YYYY-MM-DD
estimated_hours: X
---

# Task: [PBI-ID]-[N] [Task Name]

[Back to task list](tasks.md)

## Goal

[Clear, concise statement of what this task achieves]

## Context

**Parent PBI:** [PBI-ID - PBI Title](prd.md)

[Why we're doing this task - background from parent PBI]

This task is part of: [Implementation Phase Name from PRD]

## Status History

| Timestamp | Action | From Status | To Status | Details | User |
|-----------|---------|-------------|-----------|---------|------|
| 2025-10-21T10:00:00Z | Created | - | Proposed | Task created from PBI implementation plan | User |
| 2025-10-21T10:00:01Z | State Transition | Proposed | Agreed | Auto-approved (plan already approved by user) | ai-agent |

## Requirements

- [ ] Functional requirement 1
- [ ] Functional requirement 2
- [ ] Non-functional requirement (performance, security, etc.)
- [ ] Additional requirement

## Implementation Steps

1. Step 1: [Step name/description]
   - Detailed action 1
   - Detailed action 2
   - Detailed action 3

2. Step 2: [Step name/description]
   - Detailed action 1
   - Detailed action 2

3. Step 3: [Step name/description]
   - Detailed action 1
   - Detailed action 2

## Files to Modify/Create

- [ ] src/components/Feature.tsx - Add new component for feature
- [ ] src/api/endpoints.ts - Create new API endpoint
- [ ] src/types/models.ts - Add type definitions
- [ ] tests/Feature.test.tsx - Add unit tests

## Testing

**Test Cases:**
- [ ] Unit test: Component renders correctly
- [ ] Unit test: API endpoint returns expected data
- [ ] Integration test: Feature works end-to-end
- [ ] Edge case: Handle empty data
- [ ] Edge case: Handle error responses

**Coverage Target:** 80%

## Success Criteria

- [ ] Implementation complete
- [ ] All requirements met
- [ ] Tests passing
- [ ] Code follows standards
- [ ] Documentation updated

## References

**Related Tasks:**
- Depends on: 14-1 (API endpoint must be ready first)
- Blocks: 14-5 (UI needs this component)

**Additional Notes:**
- Consider performance implications for large datasets
- Follow existing component patterns
- Ensure accessibility standards (WCAG 2.1)
```

## Field Descriptions

### YAML Frontmatter

- **priority**: `high` | `medium` | `low` - Task priority level
- **created**: ISO date when task was created (YYYY-MM-DD)
- **updated**: ISO date when task was last updated (YYYY-MM-DD)
- **estimated_hours**: Estimated effort in hours, or "TBD"

### Sections

#### Goal
Single, clear statement of what this task accomplishes. Should be specific and measurable.

**Example:** "Implement user authentication API with JWT tokens"

#### Context
Explains why we're doing this task and how it relates to the parent PBI. Provides necessary background information.

#### Status History
Table tracking all state transitions:
- **Timestamp**: ISO 8601 format (YYYY-MM-DDTHH:mm:ssZ)
- **Action**: Type of event (Created, State Transition, etc.)
- **From Status**: Previous state (or "-" for creation)
- **To Status**: New state
- **Details**: Brief description of what happened
- **User**: Who performed the action

#### Requirements
Checkbox list of specific, measurable requirements. These map to PRD requirements and define what must be implemented.

**Good requirements:**
- Specific and measurable
- Testable
- Independent where possible

#### Implementation Steps
Detailed, numbered steps to implement the task. Should be specific enough for AI to follow.

**Good steps:**
- Break down into logical phases
- Include specific file paths
- Mention patterns to follow
- Note important considerations

#### Files to Modify/Create
Checkbox list of all files that will be changed or created, with brief description of what changes.

**Format:** `- [ ] path/to/file.ext - Description of changes`

#### Testing
Lists all test cases that need to be written/executed.

**Test Cases:**
- Unit tests for individual functions/components
- Integration tests for feature workflows
- Edge cases and error handling
- Performance tests if applicable

**Coverage Target:** Expected code coverage percentage

#### Success Criteria
Checkbox list of criteria that must be met for task to be considered complete.

**Standard criteria:**
- Implementation complete
- All requirements met
- Tests passing
- Code follows standards
- Documentation updated

#### References
Links to related tasks and additional notes.

**Related Tasks:**
- **Depends on:** Tasks that must be completed first
- **Blocks:** Tasks that cannot start until this completes

**Additional Notes:**
- Technical considerations
- Design decisions
- Important warnings
- Future improvements

## Usage in Workflows

### decompose-tasks
Creates task files using this template, populating fields from PRD Implementation Plan.

### execute-task
Reads task file to understand:
- What to implement (Goal, Requirements, Implementation Steps)
- Where to implement (Files to Modify/Create)
- How to test (Testing section)
- When it's done (Success Criteria)

Updates:
- Status History table (adds new rows)
- YAML frontmatter (updates `updated` field)
- Appends Implementation Notes at end

### transition-task
Updates Status History table with manual state transitions.

## Best Practices

### Writing Good Requirements
✅ "API endpoint returns user data with JWT authentication"
❌ "Make API work"

### Writing Good Implementation Steps
✅ "1. Create UserAuth.tsx component following src/components/Auth/pattern"
❌ "1. Make component"

### Writing Good Test Cases
✅ "Unit test: Login succeeds with valid credentials"
❌ "Test login"

### File Paths
- Always use relative paths from project root
- Be specific about file locations
- Include file extensions

## Example Task File

See [14-1.md](../../docs/delivery/14/14-1.md) for a complete example (if available).

## Related Documentation

- [Task Management Policy](../../docs/rules/sections/3-task-management.md)
- [Decompose Tasks Workflow](./README.md)
- [Execute Task Workflow](../execute-task/README.md)

---

**Version:** 2.0
**Last Updated:** 2025-10-21
