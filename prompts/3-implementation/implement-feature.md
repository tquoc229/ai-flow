---
name: implement-feature
phase: implementation
complexity: varies
estimated_time: varies per task
---

# Implement Feature/Task

## When To Use

- Starting implementation of a task
- Clear task definition exists
- Ready to write code

## Prerequisites

- Task ticket created and approved
- Architecture/design decided
- Development environment ready

## Context To Provide

- Task ticket content
- Related code files
- Project structure
- Code standards

## Prompt Template

```
Implement this task following project standards:

## Task Details
[Paste task ticket content or provide path: docs/delivery/tickets/tasks/task-X-Y.md]

## Project Context
Read these files for context:
- docs/project-overview.md
- docs/architecture.md
- docs/code-standards.md (if exists)

## Current Codebase
Relevant files:
[List or describe relevant existing code]

## Implementation Requirements
1. Follow existing code patterns
2. Use constants for repeated values
3. Add proper error handling
4. Write meaningful comments
5. Follow DRY, KISS, YAGNI principles

## Steps
1. Read and understand task requirements
2. Review existing code structure
3. Implement according to task plan
4. Add/update tests
5. Update documentation if needed

## Expected Deliverables
- Working code implementation
- Tests passing
- Documentation updated (if applicable)
- Summary of changes made

Follow the implementation plan in the task file.
Start implementing now.
```

## Expected Output

- Implemented code
- Passing tests
- Updated documentation
- Summary of work done

## What To Do Next

1. Review AI's implementation
2. Run tests manually
3. Make adjustments if needed
4. Use write-tests.md if more tests needed
5. Use review-code.md before committing

## Tips

- Provide full task context
- Point to existing similar code
- Specify code style preferences
- Ask AI to explain complex parts
