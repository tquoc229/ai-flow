# Policy Section 2: PBI Management

<critical>This document defines the lifecycle, states, and management rules for Product Backlog Items (PBIs).</critical>
<critical>This is Section 2 of the AI Coding Agent Project Policy.</critical>

---

## Overview

**PBI (Product Backlog Item)** is the primary unit of work in MyFlow.

- Each PBI represents a feature, improvement, or bug fix
- PBIs follow a defined lifecycle with state transitions
- All PBI changes are tracked in history
- PBIs are broken down into atomic tasks for implementation

---

## PBI Workflow States

The PBI lifecycle is defined by states in `ai-flow-config.yaml` → `pbi.states`.

| State | Description | What It Means |
|-------|-------------|---------------|
| **Proposed** | New PBI idea | PBI created with 1-line user story. Awaiting user approval of concept. |
| **PlanInReview** | Awaiting plan approval | AI generated detailed PRD. User reviews plan before proceeding. |
| **NeedsPlanRework** | Plan rejected | User rejected plan, AI must revise based on feedback. |
| **ReadyForTasks** | Plan approved | User approved PRD. Ready to decompose into tasks. |
| **InProgress** | Implementation in progress | Tasks created and being implemented. |
| **InReview** | Ready for final review | All tasks complete. Awaiting user's final approval. |
| **Done** | Completed and approved | PBI fully implemented and accepted by user. |
| **Rejected** | Failed review | PBI failed final review. Needs rework. |

### State Diagram

```
         [Proposed]
             ↓
      [PlanInReview] ←─────────┐
         ↓       ↓             │
   [ReadyForTasks] [NeedsPlanRework]
         ↓
    [InProgress] ←──────────┐
         ↓                  │
     [InReview]             │
         ↓                  │
      [Done]           [Rejected]
```

---

## Valid State Transitions

Defined in `ai-flow-config.yaml` → `pbi.transitions`:

| From | To | Trigger |
|------|-----|---------|
| Proposed | PlanInReview | User approves idea, AI generates plan |
| PlanInReview | ReadyForTasks | User approves plan |
| PlanInReview | NeedsPlanRework | User rejects plan |
| NeedsPlanRework | PlanInReview | AI revises plan |
| ReadyForTasks | InProgress | AI decomposes into tasks |
| InProgress | InReview | All tasks complete |
| InReview | Done | User approves final result |
| InReview | Rejected | User rejects final result |
| Rejected | InProgress | User reopens for fixes |

**Invalid transitions will be rejected by the system.**

---

## Critical Rules

### 1. Unique PBI IDs
- Auto-increment from last ID in backlog
- Format: Integer (1, 2, 3, ...)
- NEVER reuse IDs

### 2. Bidirectional Links
- **Backlog → PRD**: Link in backlog table
- **PRD → Backlog**: Link at bottom of PRD
- MUST maintain both directions

### 3. Legacy Discovery (MANDATORY)
- MUST perform targeted code search before creating plan
- Document findings in PRD "Legacy Discovery Findings" section
- Implementation Plan MUST prioritize code reuse over new code

### 4. History Logging
- ALL state transitions logged with ISO 8601 timestamp
- Location: `backlog.md` → PBI History section
- Format: `| {timestamp} | {pbi_id} | {action} | {details} | {user} |`

### 5. Status Source of Truth
- **backlog.md** is authoritative for PBI status
- PRD file frontmatter reflects status but backlog is primary

---

## PBI Document Structure

### Backlog Entry (`backlog.md`)

**Location:** `{output_folder}/backlog.md`

**Format:** Markdown table with columns:

| Column | Description | Required |
|--------|-------------|----------|
| ID | Unique integer | Yes |
| Actor | From user story "As a..." | Yes |
| User Story | Full user story | Yes |
| Status | Current state | Yes |
| CoS | Conditions of Satisfaction | Yes |

**Core Principles:**
- Single source of truth for all PBIs
- Ordered by priority (manual reordering allowed)
- Valid status values from `ai-flow-config.yaml`

### PRD File (`{pbi_id}/prd.md`)

**Location:** `{output_folder}/{pbi_id}/prd.md`

**Created:** When PBI transitions Proposed → PlanInReview

**Required Sections:**

1. **YAML Frontmatter**
   - status, priority, created, updated, estimated_hours

2. **Context**
   - Why (business value)
   - Current Situation
   - Desired State

3. **User Story**
   - Format: "As a [actor], I want [action], so that [benefit]"

4. **Requirements**
   - Functional Requirements (numbered list)
   - Non-Functional Requirements (performance, security, etc.)

5. **Technical Approach**
   - Architecture overview
   - **Legacy Discovery Findings** (CRITICAL SECTION)
   - Key Components
   - Tech Stack

6. **Implementation Plan**
   - High-level phases
   - Must prioritize modifying existing code (from Legacy Discovery)
   - Steps in logical order

7. **Testing Strategy**
   - Unit test categories
   - Integration test scenarios
   - E2E tests (if applicable)

8. **Success Criteria**
   - Mapped from Conditions of Satisfaction
   - Checklist format

9. **References & Notes**
   - Link back to backlog
   - Related PBIs
   - Additional notes

---

## Validation Rules

### User Story Format
- MUST include: "As a...", "I want...", "so that..."
- Must be clear and specific
- Actor must be identifiable

### Conditions of Satisfaction
- At least 1 criterion required
- Each criterion must be measurable
- No vague statements

### PRD Completeness
- All required sections present
- No placeholder text (e.g., "TODO", "TBD" except estimated_hours)
- Legacy Discovery Findings documented
- Bidirectional links valid

### State Transition Validity
- Must follow allowed transitions from config
- Cannot skip states
- History must be logged

---

## Workflows

For step-by-step execution instructions:

- **Create PBI**: `workflows/create-pbi/`
  - Proposed → PlanInReview
  - Includes Legacy Discovery
  - Generates complete PRD

- **Decompose Tasks**: `workflows/decompose-tasks/`
  - ReadyForTasks → InProgress
  - Breaks down Implementation Plan into tasks

---

## Related Sections

- [← Previous: Section 1 - Fundamentals](./1-fundamentals.md)
- [Next: Section 3 - Task Management →](./3-task-management.md)
- [← Back to Index](../project-policy-index.md)

---

**Version:** 3.0 (Refactored - Policy Only)
**Last Updated:** 2025-10-21

**Note:** For detailed workflow execution, see `workflows/` directory.
