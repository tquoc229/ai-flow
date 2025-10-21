# Create PBI Workflow

Create a new Product Backlog Item from initial idea through detailed planning with Legacy Discovery.

## Purpose

This workflow guides AI agents through:
- Creating PBI entry in backlog
- Transitioning Proposed → PlanInReview
- Performing Legacy Discovery (finding reusable code)
- Generating detailed PRD (Product Requirements Document)
- Creating bidirectional links

## When to Use

Use this workflow when:
- User has a new feature/improvement idea
- Starting a new work item
- Need formal planning documentation

## Files in This Workflow

- **workflow.yaml** - Configuration, variables, and metadata
- **instructions.md** - Step-by-step execution guide (XML structure)
- **checklist.md** - Validation checklist (planned)

## How to Invoke

### From AGENTS.md Menu
```
*create-pbi
```

### Direct Load
```
Load: workflows/create-pbi/workflow.yaml
```

## Prerequisites

- Backlog file exists
- Codebase structure documented
- Clear user story concept

## Key Features

- **Legacy Discovery**: Targeted search for reusable code
- **Auto-generated PRD**: Complete document from template
- **Bidirectional Links**: Backlog ↔ PRD
- **Priority Reuse**: Implementation plan prioritizes existing code

## Inputs Required

1. **user_story** (required)
   - Format: "As a [actor], I want [action], so that [benefit]"
   - Example: "As a developer, I want automated testing, so that I can catch bugs early"

2. **conditions_of_satisfaction** (required)
   - Measurable success criteria (list)
   - At least 1 criterion required

3. **priority** (optional, default: "Medium")
   - Options: High, Medium, Low

4. **estimated_hours** (optional, default: "TBD")

## Outputs Generated

1. **Backlog Entry** (`backlog.md`)
   - New PBI row with unique ID
   - Status: Proposed → PlanInReview
   - History entry logged

2. **PRD File** (`{pbi_id}/prd.md`)
   - Complete Product Requirements Document
   - All required sections filled
   - Legacy Discovery Findings documented
   - Bidirectional link to backlog

## Success Criteria

- ✅ PBI created with unique ID
- ✅ User story follows format
- ✅ At least 1 CoS defined
- ✅ PRD complete with all sections
- ✅ Legacy Discovery performed
- ✅ Links established
- ✅ Status = PlanInReview

## Related Workflows

- **decompose-tasks** - Next step: break down into tasks
- **execute-task** - Eventually: implement tasks

## Context Loaded

This workflow loads (~800 lines):
- AGENTS.md - Primary directive
- ai-flow-config.yaml - PBI states/transitions
- Section 2: PBI Management
- backlog.md - Current PBIs
- codebase-structure.md - For Legacy Discovery

## Estimated Duration

15-45 minutes (depends on complexity)

## Version

2.0 (BMAD-inspired structure)
