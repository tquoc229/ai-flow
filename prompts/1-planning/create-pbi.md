---
name: create-pbi
phase: planning
complexity: medium
estimated_time: 10-15 minutes
---

# Create PBI (Product Backlog Item)

## When To Use

- Starting a new feature
- Planning a major change
- Need structured requirements

## Prerequisites

- Clear understanding of user need
- Basic feature scope defined
- Priority level decided

## Context To Provide

- User pain points
- Business goals
- Technical constraints
- Existing related features

## Prompt Template

Copy and customize this prompt:

```
Create a PBI ticket following GitHub Issues style for this feature:

## Feature Overview
[Brief description of what you want to build]

## User Story
As a [user type]
I want [capability]
So that [benefit]

## Current Situation
[What's the problem now?]

## Proposed Solution
[High-level approach]

## Conditions of Satisfaction
- [ ] CoS 1: [Specific, measurable outcome]
- [ ] CoS 2: [Specific, measurable outcome]
- [ ] CoS 3: [Specific, measurable outcome]

## Technical Context
- Current stack: [technologies]
- Affected components: [list]
- Dependencies: [external packages]
- Constraints: [limitations]

## Requirements
Format as a complete PBI ticket in markdown with:
- Proper frontmatter (status, priority, dates)
- All sections filled out
- Clear acceptance criteria
- Implementation considerations

Save as: docs/delivery/[pbinumber]/pbi-X.md
```

## Expected Output

- Complete PBI ticket in markdown
- Well-structured with all sections
- Ready to save as file
- Clear enough for implementation

## What To Do Next

1. Review the generated PBI
2. Edit/refine if needed
3. Save to docs/delivery/[pbinumber]/
4. Update backlog.md with new PBI
5. Use breakdown-tasks.md prompt for next step

## Tips

- Be specific about CoS (Conditions of Satisfaction)
- Include real examples in user stories
- Mention technical constraints upfront
- Link related PBIs if any
