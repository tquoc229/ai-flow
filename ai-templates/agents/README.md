# Agent Definitions

## What Are Agents?

Agents are **specialized roles** that you can assign to your AI assistant. Each agent has:

- Specific expertise
- Clear responsibilities
- Defined workflow
- Output standards

## How To Use Agents

### Method 1: Direct Use (Simple)

1. Read agent definition file (e.g., `agents/planner.md`)
2. Copy its content
3. Paste into your AI chat with: "You are the [Agent Name]. [paste definition]"
4. Give the task
5. AI follows agent guidelines

### Method 2: Use Agent Prompts (Easier)

1. Go to `.prompts/agents/`
2. Find the agent prompt (e.g., `use-planner.md`)
3. Follow instructions in that prompt
4. It tells you how to invoke the agent

## Available Agents

| Agent               | Purpose                            | When To Use                                 |
| ------------------- | ---------------------------------- | ------------------------------------------- |
| **brainstormer**    | Architecture & solution evaluation | Before planning, need to compare approaches |
| **planner**         | Research & implementation planning | Start of feature, need detailed plan        |
| **researcher**      | Technical research                 | Need to investigate technologies            |
| **ui-ux-designer**  | UI/UX design                       | Any UI work needed                          |
| **debugger**        | Debug & troubleshoot               | Bugs, errors, issues                        |
| **database-admin**  | Database work                      | Schema, optimization, queries               |
| **tester**          | Testing & validation               | After implementation                        |
| **code-reviewer**   | Code quality review                | Before committing                           |
| **docs-manager**    | Documentation                      | Update/create docs                          |
| **project-manager** | Project oversight                  | Track progress, status                      |

## Workflow Integration

```
Planning Phase:
  └─ brainstormer → planner → researcher (if needed)

Design Phase:
  └─ ui-ux-designer (for UI)
  └─ database-admin (for database)

Implementation Phase:
  └─ [You code] or [AI assists]
  └─ debugger (if issues)

Testing Phase:
  └─ tester → code-reviewer

Integration:
  └─ project-manager → docs-manager
```

## Tips

- You don't HAVE to use agents
- Use them when you need expertise
- Mix and match as needed
- Agents help structure AI's thinking
- You're always in control
