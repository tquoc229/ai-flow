---
name: use-planner
phase: planning
agent: planner
estimated_time: 20-30 minutes
---

# Use Planner Agent

## When To Use

- Need comprehensive implementation plan
- Starting complex feature
- Need research and design

## How To Use

### Step 1: Read Agent Definition

First, read the planner agent definition:

```
cat .agents/planner.md
```

### Step 2: Prepare Context

Gather:

- PBI details
- Project docs (architecture, standards)
- Similar implementations (if any)

### Step 3: Compose Prompt

```
You are the Planner Agent.

[Paste content from .agents/planner.md]

---

## Current Task
Create a comprehensive implementation plan for:

[Paste PBI details]

## Project Context
- Read: docs/project-overview.md
- Read: docs/architecture.md
- Current codebase: [describe or use repomix output]

## Requirements
Follow your agent definition to:
1. Research the topic (spawn researchers if needed conceptually)
2. Analyze approaches
3. Create detailed implementation plan
4. Break into tasks
5. Document everything

Output:
- Implementation plan: plans/YYMMDD-feature-name-plan.md
- Task breakdown
- Research findings

Begin planning now.
```

### Step 4: Execute

1. Copy prompt to your AI tool
2. Let AI work
3. Review outputs
4. Save plans to correct locations

### Step 5: Follow Up

- Review plan thoroughly
- Adjust if needed
- Use breakdown-tasks.md next

## Tips

- Agent definitions are like "roles"
- You tell AI to "be" that agent
- Provide agent definition in prompt
- Agent follows its own guidelines
