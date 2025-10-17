# Agent Definitions

## What Are Agents?

Agents are **specialized roles** for your AI assistant. Each agent has:
- Specific expertise
- Clear responsibilities
- Defined workflow
- Output standards

## How To Use

```
"Bạn là [Agent Name] (đọc .agents/[agent].md).
Thực hiện docs/tickets/[ticket].md"
```

## Available Agents

| Agent | Purpose |
|-------|---------|
| **planner** | Planning & research |
| **ui-ux-designer** | UI/UX design |
| **debugger** | Troubleshooting |
| **tester** | Testing & validation |
| **code-reviewer** | Code quality |

## Example

```
"Bạn là UI/UX Designer (đọc .agents/ui-ux-designer.md).
Thực hiện docs/tickets/design-notification-ui.md"
```
