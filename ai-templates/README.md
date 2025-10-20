# Ticket Templates Guide

## Template Structure

```markdown
---
type: feature|bug|task|refactor|design|research
status: todo|in-progress|review|done
priority: high|medium|low
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# [Ticket Title]

## Context

[Why this ticket exists]

## Goal

[What we want to achieve]

## Requirements

[Specific requirements]

## Implementation Notes

[How to approach this]

## Success Criteria

[How to know it's done]

## References

[Links to docs, examples, etc.]
```

## Quick Start

1. **Choose template** based on work type
2. **Copy to docs/tickets/**

```bash
   cp docs/templates/feature.md docs/tickets/feature-my-feature.md
```

3. **Fill in details** - replace all [placeholders]
4. **Execute ticket**

```
   "Thực hiện docs/tickets/feature-my-feature.md cho tôi"
```

## When To Use Each Template

| Template        | Use When                   | Example                                                    |
| --------------- | -------------------------- | ---------------------------------------------------------- |
| **feature.md**  | Building new functionality | "Add user authentication", "Implement payment gateway"     |
| **bug.md**      | Fixing issues              | "Login button not working", "Database timeout error"       |
| **task.md**     | Simple implementation work | "Add validation", "Update config", "Refactor function"     |
| **design.md**   | UI/UX work needed          | "Design dashboard", "Create notification component"        |
| **research.md** | Need to investigate        | "Compare GraphQL libraries", "Research caching strategies" |
| **refactor.md** | Improve existing code      | "Refactor auth module", "Optimize database queries"        |

## Template Sections Explained

### Required Sections (Fill These)

- **Title**: Clear, descriptive name
- **Context**: Why this work exists
- **Goal**: What we want to achieve
- **Requirements**: Specific, testable requirements
- **Success Criteria**: How to know it's done

### Optional Sections (Use If Helpful)

- **Implementation Notes**: Technical details
- **References**: Links to docs, examples
- **Testing**: Test cases and strategy

## Tips for Good Tickets

✅ **DO:**

- Be specific and clear
- Include examples
- List all requirements
- Define success criteria
- Link to relevant docs
- Provide context

❌ **DON'T:**

- Be vague or ambiguous
- Assume knowledge
- Skip success criteria
- Forget to update status
- Leave placeholders unfilled

## AI Execution Patterns

### Simple Pattern

```
"Thực hiện docs/tickets/task-add-validation.md cho tôi"
```

### With Context

```
"Đọc docs/project-overview.md và docs/tickets/feature-graphql.md
rồi thực hiện ticket này"
```

### With Agent Role

```
"Bạn là UI/UX Designer Agent (đọc .agents/ui-ux-designer.md).
Thực hiện docs/tickets/design-notification-ui.md"
```

### With Specific Instructions

```
"Thực hiện docs/tickets/bug-login-timeout.md
Chú ý: Sử dụng retry logic với exponential backoff"
```

## Workflow Example

```bash
# 1. Copy template
cp docs/templates/feature.md docs/tickets/feature-real-time-chat.md

# 2. Edit ticket
vim docs/tickets/feature-real-time-chat.md
# ... fill in all details

# 3. Execute with AI
# Tell your AI assistant:
"Thực hiện docs/tickets/feature-real-time-chat.md cho tôi"

# 4. Review & test
# ... check AI's output

# 5. Move to completed when done
mv docs/tickets/feature-real-time-chat.md \
   docs/completed/2025-01/feature-real-time-chat.md
```

## File Naming Convention

```
[type]-[short-description].md

Examples:
✅ feature-graphql-api.md
✅ bug-login-timeout.md
✅ task-add-tests.md
✅ design-dashboard-ui.md
✅ research-caching-solutions.md

❌ myfeature.md
❌ bug1.md
❌ todo.md
```
