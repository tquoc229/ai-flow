---
name: review-code
phase: testing
complexity: medium
estimated_time: 10-15 minutes
---

# Review Code Quality

## When To Use

- After implementing a task
- Before committing code
- Want quality check

## Prerequisites

- Code implementation complete
- Tests written and passing
- Changes ready to commit

## Context To Provide

- What was changed
- What files affected
- What to focus on

## Prompt Template

```
Review the code changes I just made:

## Changes Overview
Task: [task ID and description]
Files changed: [list files]

## Review Focus
Please review for:
1. **Code Quality**
   - Follows project standards
   - Clean and readable
   - Proper naming conventions
   - No code smells

2. **Functionality**
   - Meets requirements
   - Handles edge cases
   - Proper error handling

3. **Testing**
   - Adequate test coverage
   - Tests are meaningful
   - No flaky tests

4. **Documentation**
   - Code comments clear
   - API docs updated (if needed)
   - README updated (if needed)

5. **Security & Performance**
   - No security issues
   - No obvious performance problems

## Project Standards
[Link to or describe code standards]

## Output Format
Provide review as:
- Overall assessment (Ready/Needs work)
- Critical issues (if any)
- Suggestions for improvement
- What's done well

Be constructive and specific.
```

## Expected Output

- Code review report
- List of issues (by priority)
- Specific recommendations
- Positive observations

## What To Do Next

1. Address critical issues
2. Consider suggestions
3. Make necessary changes
4. Commit with proper message
5. Update task status

## Tips

- Review in small chunks
- Focus on important issues
- Don't be too pedantic
- Acknowledge good code
