# MyFlow Workflows Directory

BMAD-inspired workflow organization for AI-native task-driven development.

## Structure

Each workflow is organized in its own directory with standard files:

```
workflows/
├── README.md (this file)
├── workflow-name/
│   ├── workflow.yaml       # Configuration, variables, metadata
│   ├── instructions.md     # Step-by-step execution guide (XML structure)
│   ├── checklist.md        # Validation checklist (optional)
│   └── README.md           # Workflow documentation
```

## Available Workflows

### PBI Management

#### 1. **create-pbi/**
Create new Product Backlog Item with Legacy Discovery and detailed PRD.

- **Purpose:** Start new work item from idea to plan
- **Input:** User story, Conditions of Satisfaction
- **Output:** Backlog entry + PRD file
- **Duration:** 15-45 minutes
- **Command:** `*create-pbi`

**Key Features:**
- Legacy Discovery (finds reusable code)
- Auto-generated PRD with all sections
- Bidirectional links (Backlog ↔ PRD)

---

#### 2. **decompose-tasks/**
Break down approved PBI plan into executable tasks.

- **Purpose:** Convert PRD Implementation Plan → concrete tasks
- **Input:** PBI ID (status must be ReadyForTasks)
- **Output:** tasks.md index + individual task files
- **Duration:** 10-30 minutes
- **Command:** `*decompose-tasks`

**Key Features:**
- Auto-approval (all tasks → Agreed)
- Atomic task creation
- Complete task structure

---

### Task Management

#### 3. **execute-task/**
Execute a task from Agreed to InReview with full implementation and testing.

- **Purpose:** Implement task requirements with AI guidance
- **Input:** Task path (task must be Agreed)
- **Output:** Implemented code + tests + documentation
- **Duration:** 30-120 minutes
- **Command:** `*execute-task`

**Key Features:**
- AI Elicitation (suggests 3 approaches per requirement)
- Strict validation (100+ checklist items)
- Automatic state transitions
- Optional git commit

---

## Workflow Lifecycle

```
User Idea
    ↓
[create-pbi]
    ↓
PBI with PRD (status: PlanInReview)
    ↓
User approves → ReadyForTasks
    ↓
[decompose-tasks]
    ↓
Tasks created (status: Agreed)
    ↓
For each task:
    [execute-task]
        ↓
    Task complete (status: InReview)
    User reviews → Done
    ↓
All tasks Done
    ↓
PBI complete (status: Done)
```

## How AI Agents Use Workflows

### 1. Load workflow.yaml
```yaml
# Extract standard config block
user_name: "User"
communication_language: "Vietnamese"
output_folder: "{project-root}/docs/delivery"
date: system-generated
```

### 2. Load recommended_inputs
```yaml
recommended_inputs:
  - AGENTS.md
  - ai-flow-config.yaml
  - Section X (specific to workflow)
  - Context files
```

### 3. Execute instructions.md
```xml
<workflow>
  <step n="1" goal="...">
    <action>...</action>
    <ask>...</ask>
    <check>...</check>
  </step>
</workflow>
```

### 4. Validate with checklist.md
- Run validation items
- Calculate pass rate
- Report to user

## Context Loading Optimization

**Problem:** Loading entire policy (3000+ lines) for every workflow wastes tokens.

**Solution:** Each workflow loads only what it needs:

| Workflow | Context Loaded | Lines | Savings |
|----------|----------------|-------|---------|
| create-pbi | PBI Management only | ~800 | 70%+ |
| decompose-tasks | Task Management only | ~1000 | 65%+ |
| execute-task | Task + Testing | ~1500 | 50% |

## Configuration

Workflows are registered in `ai-flow-config.yaml`:

```yaml
workflows:
  execute_task: "{workflows_root}/execute-task/workflow.yaml"
  create_pbi: "{workflows_root}/create-pbi/workflow.yaml"
  decompose_tasks: "{workflows_root}/decompose-tasks/workflow.yaml"
```

## Standard Config Block

All workflows share these variables from `ai-flow-config.yaml`:

```yaml
user_name: "User"
communication_language: "Vietnamese"
document_output_language: "Vietnamese"
output_folder: "{project-root}/docs/delivery"
date: system-generated
```

## Variable Substitution

Throughout workflows, use `{variable}` syntax:

- `{user_name}` → User name from config
- `{communication_language}` → Language for AI responses
- `{output_folder}` → Where to write outputs
- `{project-root}` → Project root directory
- `{pbi_id}`, `{task_id}` → Extracted from paths

## Creating New Workflows

1. **Create directory:** `workflows/new-workflow/`

2. **Create workflow.yaml:**
```yaml
name: new-workflow
description: "..."
version: "2.0"

# Standard config block
config_source: "{project-root}/ai-flow-config.yaml"
output_folder: "{config_source}:output_folder"
user_name: "{config_source}:user_name"
communication_language: "{config_source}:communication_language"
date: system-generated

# Component files
installed_path: "{project-root}/workflows"
instructions: "{installed_path}/new-workflow/instructions.md"
validation: "{installed_path}/new-workflow/checklist.md"
template: false

# ... rest of config
```

3. **Create instructions.md:**
```xml
<critical>...</critical>

<workflow>
  <step n="1" goal="...">
    <action>...</action>
  </step>
</workflow>
```

4. **Create README.md** (document the workflow)

5. **Register in ai-flow-config.yaml:**
```yaml
workflows:
  new_workflow: "{workflows_root}/new-workflow/workflow.yaml"
```

6. **Add to AGENTS.md menu:**
```xml
<item cmd="*new-workflow" workflow="{project-root}/workflows/new-workflow/workflow.yaml">
  Description of workflow
</item>
```

## Best Practices

1. **One Workflow, One Purpose**
   - Each workflow should do ONE thing well
   - Avoid mega-workflows that do everything

2. **Minimize Context Loading**
   - Load only relevant policy sections
   - Don't load entire policy for simple workflows

3. **Use Standard Config Block**
   - Always include the 5 standard variables
   - Allows portability and consistency

4. **XML Structure for Instructions**
   - Use `<workflow>`, `<step>`, `<action>` tags
   - Makes parsing easier for AI

5. **Document Everything**
   - README.md explains purpose and usage
   - workflow.yaml has clear comments
   - instructions.md includes examples

## Related Documentation

- **[AGENTS.md](../AGENTS.md)** - Primary AI agent directive
- **[ai-flow-config.yaml](../ai-flow-config.yaml)** - Central configuration
- **[docs/rules/](../docs/rules/)** - Policy sections

## Version

2.0 (BMAD-inspired structure)

---

**Questions?** Check individual workflow README files or consult policy sections.
