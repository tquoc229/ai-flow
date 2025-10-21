# PRD (Product Requirements Document) Template

This is the standard template used by the `*create-pbi` workflow when creating PRD files.

## Template Structure

```markdown
---
status: PlanInReview
priority: medium
created: YYYY-MM-DD
updated: YYYY-MM-DD
estimated_hours: TBD
---

# [PBI Title/Name]

## Context

**Why do we need this?**
[Explain the problem or opportunity]

**Current Situation:**
[What exists now]

**Desired State:**
[What we want]

## User Story

As a [user type]
I want [capability]
So that [benefit]

**Example Scenario:**

```
Given [precondition]
When [user action]
Then [expected outcome]
```

## Requirements

### Functional Requirements

- [ ] REQ-1: [Specific requirement]
- [ ] REQ-2: [Specific requirement]
- [ ] REQ-3: [Specific requirement]

### Non-Functional Requirements

- [ ] Performance: [e.g., Response time < 200ms]
- [ ] Security: [e.g., Requires authentication]
- [ ] Scalability: [e.g., Support 1000 concurrent users]

## Technical Approach

**Architecture:**

```
[Describe or diagram the architecture]
```

---

### **Legacy Discovery Findings**

[**AI_Agent MUST FILL THIS SECTION.** Report on existing files, functions, APIs, or components relevant to this PBI, adhering to the `Legacy Code Prioritization` principle.]

- **File:** `path/to/relevant/file.ts`
  - **Function/API:** `functionName()`
  - **Notes:** [Will be reused/extended for...]
- **Component:** `path/to/component.vue`
  - **Notes:** [Will be reused...]

---

**Key Components (To build/modify):**
[List new components to build OR existing components to modify, based on "Discovery Findings" above.]

1. Component A: [Purpose]
2. Component B: [Purpose]
3. Component C: [Purpose]

**Technology Stack:**

- Frontend: [technologies]
- Backend: [technologies]
- Database: [technologies]
- External Services: [APIs, libraries]

## Implementation Plan

### Phase 1: [Phase Name]

1. Step 1: [Description]
   - Files: [list files to create/modify]
   - Estimated: [time]

2. Step 2: [Description]
   - Files: [list files]
   - Estimated: [time]

### Phase 2: [Phase Name]

...

## Testing Strategy

**Unit Tests:**
- Test case 1
- Test case 2

**Integration Tests:**
- Test scenario 1
- Test scenario 2

**E2E Tests:**
- User flow 1
- User flow 2

## Success Criteria

- [ ] All functional requirements met
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Performance benchmarks met

## References

- [Link to design doc]
- [Link to API spec]
- [Link to similar implementation]
- [Back to Backlog](../backlog.md)

## Notes

[Any additional notes, constraints, or considerations]
```

## Field Descriptions

### YAML Frontmatter

- **status**: Current PBI state (PlanInReview, ReadyForTasks, InProgress, etc.)
- **priority**: `high` | `medium` | `low` - PBI priority level
- **created**: ISO date when PBI was created (YYYY-MM-DD)
- **updated**: ISO date when PBI was last updated (YYYY-MM-DD)
- **estimated_hours**: Total estimated effort, or "TBD"

### Sections

#### Context

**Why do we need this?**
Explains the business problem or opportunity. What pain point are we solving? What value will this bring?

**Good example:**
> "Users currently cannot track their progress across multiple projects, leading to missed deadlines and poor visibility into team capacity."

**Current Situation:**
Describes what exists today. What are users doing now? What limitations exist?

**Desired State:**
Describes the target outcome. What will be possible after implementation?

#### User Story

Standard format with three components:
- **As a [user type]**: Who is this for?
- **I want [capability]**: What do they need?
- **So that [benefit]**: Why do they need it?

**Example Scenario:**
Concrete example using Given/When/Then format (BDD style).

```
Given I have 3 active projects
When I view my dashboard
Then I see progress indicators for each project
```

#### Requirements

##### Functional Requirements
Checkbox list of specific, testable functional requirements. Use REQ-1, REQ-2, etc. numbering.

**Good requirements:**
- Specific and measurable
- Testable
- User-focused
- Independent where possible

**Examples:**
- [ ] REQ-1: User can create new project with name, description, and due date
- [ ] REQ-2: System validates project name is unique within organization
- [ ] REQ-3: User receives email confirmation when project is created

##### Non-Functional Requirements
Checkbox list covering performance, security, scalability, etc.

**Categories:**
- Performance: Response times, throughput, resource usage
- Security: Authentication, authorization, data protection
- Scalability: Concurrent users, data volume limits
- Usability: Accessibility, mobile support
- Reliability: Uptime, error handling

#### Technical Approach

**Architecture:**
High-level description or diagram of the solution architecture. Can include:
- System components
- Data flow
- External integrations
- Technology choices

**Legacy Discovery Findings:**
**CRITICAL SECTION** - AI must document all existing code that can be reused.

Format:
- **File:** Path to existing file
- **Function/API:** Specific function/API name
- **Notes:** How it will be reused/extended

This section enforces the "Legacy Code Prioritization" principle - always reuse before creating new.

**Key Components (To build/modify):**
Lists all components based on Legacy Discovery:
1. Existing components to modify (from Discovery)
2. New components to build (only when no existing solution)

**Technology Stack:**
List of technologies organized by layer:
- Frontend: React, Vue, Angular, etc.
- Backend: Node.js, Python, Java, etc.
- Database: PostgreSQL, MongoDB, etc.
- External Services: Third-party APIs, libraries

#### Implementation Plan

Breaks down implementation into logical phases.

**Phase Structure:**
```markdown
### Phase 1: [Phase Name]

1. Step 1: [Description]
   - Files: [list files to create/modify]
   - Estimated: [time estimate]

2. Step 2: [Description]
   - Files: [list files]
   - Estimated: [time estimate]
```

**Good phases:**
- Logical groupings (e.g., "Database Setup", "API Implementation", "UI Components")
- Sequential when dependencies exist
- Each phase has clear deliverable
- Includes file paths and time estimates

#### Testing Strategy

Defines comprehensive testing approach across three levels:

**Unit Tests:**
Individual function/component tests
- Focus on single units in isolation
- Fast execution
- High coverage

**Integration Tests:**
Multi-component interaction tests
- API + Database
- Component + Service
- External API integration

**E2E Tests:**
Complete user workflow tests
- User registration flow
- Purchase flow
- Data export flow

#### Success Criteria

Checkbox list of criteria that must be met for PBI to be considered complete.

**Standard criteria:**
- [ ] All functional requirements met
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Performance benchmarks met

Additional criteria from Conditions of Satisfaction are mapped here.

#### References

Links to related documentation:
- Design documents
- API specifications
- Similar implementations
- External resources
- Link back to backlog

#### Notes

Any additional information:
- Technical constraints
- Design decisions
- Important warnings
- Future improvements
- Known limitations

## Usage in Workflows

### create-pbi
Creates PRD using this template, populating all sections from:
- User story and CoS
- Legacy Discovery results
- AI-generated requirements and plan

### approve-pbi-plan
Reviews PRD content for user approval/rejection.

### decompose-tasks
Reads Implementation Plan to break into tasks.

### execute-task
References PRD for context and technical approach.

## Best Practices

### Writing Good Context

✅ **Good:**
> "Why: Manual data entry is error-prone and time-consuming
> Current: Users manually copy data from emails into spreadsheet
> Desired: Automated data extraction from email attachments"

❌ **Bad:**
> "Why: Need better data handling
> Current: Not great
> Desired: Make it better"

### Writing Good Requirements

✅ **Good:**
- [ ] REQ-1: API endpoint `/api/projects` accepts POST with name, description, due_date
- [ ] REQ-2: System returns 400 error if required fields missing
- [ ] REQ-3: Response includes created project with auto-generated ID

❌ **Bad:**
- [ ] Make API work
- [ ] Handle errors
- [ ] Return data

### Legacy Discovery Best Practices

**Always document:**
- Exact file paths
- Function/API names
- How they will be reused

**Example:**
```markdown
- **File:** `src/api/user-service.ts`
  - **Function/API:** `authenticateUser(credentials)`
  - **Notes:** Will be extended to support OAuth2 in addition to password auth

- **Component:** `src/components/DataTable.vue`
  - **Notes:** Will be reused for project list with custom columns config
```

### Implementation Plan Best Practices

**Break into phases:**
- Phase 1: Foundation (database, core models)
- Phase 2: Backend API
- Phase 3: Frontend UI
- Phase 4: Integration & Testing

**Include estimates:**
```markdown
1. Create database schema
   - Files: migrations/001_create_projects.sql
   - Estimated: 2 hours
```

## Example Scenario Template

Use this format for Example Scenarios:

```markdown
**Example Scenario:**

```
Given [precondition - what state the system is in]
When [user action - what the user does]
Then [expected outcome - what should happen]
```

**Examples:**

```
Given I am logged in as a project manager
When I click "Create New Project" and fill in the form
Then I see the new project in my dashboard
```

```
Given I have 5 projects with overdue tasks
When I view my notifications
Then I see alerts for each overdue project
```
```

## Related Documentation

- [PBI Management Policy](../../docs/rules/sections/2-pbi-management.md)
- [Create PBI Workflow](./README.md)
- [Task Template](../decompose-tasks/TASK_TEMPLATE.md)

---

**Version:** 2.0
**Last Updated:** 2025-10-21
