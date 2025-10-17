---
name: breakdown-tasks
phase: planning
complexity: medium
estimated_time: 15-20 minutes
---

# Breakdown PBI into Tasks

## When To Use

- After PBI is created and approved
- Before starting implementation
- Need granular work items

## Prerequisites

- PBI ticket exists and is clear
- Technical approach decided
- Have read project architecture docs

## Context To Provide

- Link to PBI ticket
- Architecture constraints
- Team velocity
- Preferred task size

## Prompt Template

```
I have this PBI that needs to be broken down into implementable tasks:

## PBI Details
[Paste or link to PBI content]

## Project Context
- Tech stack: [list]
- Architecture: [describe or link to docs/architecture.md]
- Code standards: [link to standards doc]

## Requirements for Task Breakdown
1. Break into 5-10 tasks (each 2-4 hours)
2. Include:
   - Research/planning tasks (if needed)
   - Implementation tasks
   - Testing tasks
   - Documentation tasks
3. Each task should:
   - Have clear input/output
   - Be independently testable
   - Follow project structure

## Task Format
For each task, create a markdown file with:
- Clear description
- Requirements
- Implementation steps
- Test plan (proportional to complexity)
- Files to modify/create
- Definition of Done

Generate task tickets as:
- task-X-1.md
- task-X-2.md
- ...
- task-X-N.md

Also create a tasks summary table in tasks.md
```

## Expected Output

- Individual task files (markdown)
- Task summary table
- Clear dependency order
- Estimated time per task

## What To Do Next

1. Review task breakdown
2. Adjust if tasks too large/small
3. Save task files
4. Pick first task to implement
5. Use implement-feature.md prompt

## Tips

- Keep tasks focused and small
- Include "E2E CoS Test" as final task
- Consider parallel vs sequential tasks
- Add research tasks for new technologies
