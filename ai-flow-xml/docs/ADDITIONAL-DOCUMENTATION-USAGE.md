# Additional Documentation Usage Pattern

**Version:** 1.0
**Last Updated:** 2025-10-21
**Purpose:** Guide AI agents on WHEN and HOW to use Additional Documentation files

---

## Overview

MyFlow skeleton includes **6 critical documentation files** that help AI agents understand legacy projects and avoid duplicate work:

1. **Reusable-Components.md** - Existing components, utilities, shared modules
2. **api-docs.md** - API endpoints, contracts, data models
3. **integration-guide.md** - Third-party integrations, authentication
4. **codebase-structure-architecture-code-standards.md** - Architecture and standards
5. **project-overview.md** - Business context, user personas, vision
6. **DesignGuideline.md** - UI/UX patterns, design system

These files are defined in `config.yaml` under `locations:` section.

---

## Why These Files Matter

### Problem Without Them:
- ❌ AI creates duplicate components that already exist
- ❌ AI breaks existing API contracts
- ❌ AI uses inconsistent design patterns
- ❌ AI doesn't understand business context
- ❌ AI recreates integrations that already work

### Solution With Them:
- ✅ AI reuses existing components (faster development)
- ✅ AI follows established API patterns (no breaking changes)
- ✅ AI maintains design consistency (better UX)
- ✅ AI makes context-aware decisions (better features)
- ✅ AI leverages existing integrations (no duplicate work)

---

## Loading Strategy: Context-Aware

**Key Principle:** Load ONLY what's relevant to the current task to optimize context usage.

### Phase-Based Loading

#### **Phase 1: ALWAYS Load (Core Context)**
```yaml
core_context:
  - AGENTS.md              # Primary directive
  - ai-flow-config.yaml            # Configuration
  - project-policy-index.md # Policy overview
```

#### **Phase 2: WORKFLOW-Specific**
```yaml
workflow_context:
  - Section 2 (for PBI workflows)
  - Section 3 (for Task workflows)
  - Section 4 (for Testing)
```

#### **Phase 3: TASK-Specific (Conditional)**
```yaml
supporting_docs:
  - Load based on task requirements
  - Use "when" conditions to filter
```

---

## When to Load Each File

### 1. **Reusable-Components.md**

**Load When:**
- Creating new functions/utilities
- Building new UI components
- Implementing shared logic
- Task mentions "component", "utility", "helper", "shared"

**Purpose:**
- Identify existing reusable code
- Avoid duplicating utilities
- Leverage shared modules

**Example Scenario:**
```yaml
Task: "Create a date formatting utility"

AI should:
1. Load Reusable-Components.md FIRST
2. Search for existing date utilities
3. If found → reuse existing
4. If not found → create new and document
```

---

### 2. **api-docs.md**

**Load When:**
- Creating new API endpoints
- Modifying data models
- Implementing authentication
- Task mentions "API", "endpoint", "REST", "GraphQL"

**Purpose:**
- Follow existing API patterns
- Maintain consistent data contracts
- Avoid breaking changes

**Example Scenario:**
```yaml
Task: "Add user profile endpoint"

AI should:
1. Load api-docs.md
2. Check existing user-related endpoints
3. Follow same authentication pattern
4. Use consistent response format
5. Document new endpoint
```

---

### 3. **integration-guide.md**

**Load When:**
- Integrating third-party services
- Implementing OAuth/authentication
- Connecting to external APIs
- Task mentions "integration", "third-party", "OAuth", "webhook"

**Purpose:**
- Reuse existing integration patterns
- Follow established auth flows
- Leverage configured services

**Example Scenario:**
```yaml
Task: "Add email notifications"

AI should:
1. Load integration-guide.md
2. Check if email service already configured
3. Reuse existing email client
4. Follow same error handling pattern
```

---

### 4. **codebase-structure-architecture-code-standards.md**

**Load When:**
- Implementing any new code
- Creating new modules/files
- Refactoring existing code
- Always for non-trivial tasks

**Purpose:**
- Follow project architecture
- Use correct folder structure
- Apply coding standards

**Example Scenario:**
```yaml
Task: "Add new feature module"

AI should:
1. Load codebase-structure.md
2. Identify correct folder location
3. Follow module naming convention
4. Apply architectural patterns
```

---

### 5. **project-overview.md**

**Load When:**
- Creating new features
- Making product decisions
- Understanding user needs
- Task requires business context

**Purpose:**
- Understand business goals
- Know target users
- Make context-aware decisions

**Example Scenario:**
```yaml
Task: "Improve onboarding flow"

AI should:
1. Load project-overview.md
2. Understand user personas
3. Align with product vision
4. Design appropriate UX
```

---

### 6. **DesignGuideline.md**

**Load When:**
- Creating UI components
- Styling pages/elements
- Implementing user interactions
- Task mentions "UI", "UX", "design", "styling", "component"

**Purpose:**
- Maintain design consistency
- Follow design system
- Use correct component library

**Example Scenario:**
```yaml
Task: "Create settings page"

AI should:
1. Load DesignGuideline.md
2. Use design system colors
3. Follow spacing/typography rules
4. Reuse existing UI components
```

---

## Implementation in Workflows

### create-pbi/workflow.yaml

```yaml
recommended_inputs:
  # Phase 3: Load project documentation (for Legacy Discovery)
  project_docs:
    - path: "{project-root}/docs/Reusable-Components.md"
      when: "when creating plan AND feature involves UI/components"

    - path: "{project-root}/docs/api-docs.md"
      when: "when creating plan AND feature involves backend/API"

    - path: "{project-root}/docs/integration-guide.md"
      when: "when creating plan AND feature involves external integrations"

    - path: "{project-root}/docs/DesignGuideline.md"
      when: "when creating plan AND feature involves user interface"
```

### execute-task/workflow.yaml

```yaml
recommended_inputs:
  # Phase 3: Load supporting documentation (as needed)
  supporting_docs:
    - path: "docs/Reusable-Components.md"
      when: "if task involves creating new functions/components/utilities"

    - path: "docs/api-docs.md"
      when: "if task involves API endpoints or data models"

    - path: "docs/integration-guide.md"
      when: "if task involves external services or integrations"

    - path: "docs/DesignGuideline.md"
      when: "if task involves user interface or styling"
```

---

## AI Agent Decision Tree

```
Start Task
    ↓
Does task involve NEW code creation?
    NO → Skip Additional Documentation
    YES ↓

What type of code?

    UI/Components?
        → Load: DesignGuideline.md
        → Load: Reusable-Components.md

    API/Backend?
        → Load: api-docs.md
        → Load: Reusable-Components.md

    Integrations?
        → Load: integration-guide.md
        → Load: api-docs.md

    General Code?
        → Load: codebase-structure.md
        → Load: Reusable-Components.md

Need business context?
    YES → Load: project-overview.md
    NO → Skip
```

---

## Best Practices for AI Agents

### 1. **Load Conditionally, Not Always**
```yaml
# ❌ BAD - Always load everything
recommended_inputs:
  - Reusable-Components.md  # Always
  - api-docs.md             # Always
  - DesignGuideline.md      # Always

# ✅ GOOD - Load based on task
recommended_inputs:
  - Reusable-Components.md  # if creating new code
  - api-docs.md             # if API-related task
  - DesignGuideline.md      # if UI-related task
```

### 2. **Check Before Creating**
```
Before creating NEW component:
1. Load Reusable-Components.md
2. Search for similar functionality
3. If exists → reuse
4. If not → create and document
```

### 3. **Update After Creating**
```
After creating NEW component:
1. Document in Reusable-Components.md
2. Update API docs (if applicable)
3. Update integration guide (if applicable)
```

### 4. **Keyword Triggers**

AI should auto-load files when task contains these keywords:

| File | Trigger Keywords |
|------|------------------|
| **Reusable-Components.md** | component, utility, helper, shared, function, module |
| **api-docs.md** | API, endpoint, REST, GraphQL, route, controller |
| **integration-guide.md** | integration, third-party, OAuth, webhook, external |
| **codebase-structure.md** | (always for code tasks) |
| **project-overview.md** | feature, product, user, business, requirement |
| **DesignGuideline.md** | UI, UX, design, component, styling, layout |

---

## Context Optimization

### Token Usage Estimation

| File | Estimated Lines | Tokens (approx) |
|------|----------------|-----------------|
| Reusable-Components.md | 200-500 | 800-2000 |
| api-docs.md | 300-800 | 1200-3200 |
| integration-guide.md | 200-400 | 800-1600 |
| codebase-structure.md | 300-600 | 1200-2400 |
| project-overview.md | 150-300 | 600-1200 |
| DesignGuideline.md | 400-800 | 1600-3200 |

**Total if load ALL:** ~6000-13600 tokens

**Smart loading (1-2 files):** ~1500-5000 tokens

**Savings:** 60-75% context reduction

---

## Examples

### Example 1: UI Task

```yaml
Task: "Create user profile card component"

AI loads:
1. DesignGuideline.md      # Design system patterns
2. Reusable-Components.md  # Check for existing card component
3. codebase-structure.md   # Component folder location

AI skips:
- api-docs.md              # Not creating API
- integration-guide.md     # Not integrating external service
- project-overview.md      # Context already clear
```

### Example 2: API Task

```yaml
Task: "Add payment processing endpoint"

AI loads:
1. api-docs.md             # Follow API patterns
2. integration-guide.md    # Payment gateway integration
3. Reusable-Components.md  # Check for payment utilities
4. codebase-structure.md   # API folder structure

AI skips:
- DesignGuideline.md       # No UI changes
- project-overview.md      # Context already clear
```

### Example 3: New Feature

```yaml
Task: "Implement user onboarding flow"

AI loads:
1. project-overview.md     # Understand user personas
2. DesignGuideline.md      # UI patterns
3. Reusable-Components.md  # Check for onboarding components
4. api-docs.md             # User data APIs
5. codebase-structure.md   # Feature folder structure

Comprehensive load for major feature
```

---

## Validation Checklist

Before marking task complete, AI should verify:

```markdown
[ ] Checked Reusable-Components.md for existing code (if applicable)
[ ] Followed DesignGuideline.md patterns (if UI task)
[ ] Maintained API contracts from api-docs.md (if API task)
[ ] Followed codebase-structure.md architecture
[ ] Updated relevant documentation with new additions
```

---

## Future Enhancements

### Planned Improvements:
1. Auto-detect task type from keywords
2. Pre-filter documentation sections
3. Create documentation snippets
4. Build documentation index for faster search

---

## Related Files

- `ai-flow-config.yaml` - Defines all documentation paths
- `workflows/create-pbi/workflow.yaml` - Uses during Legacy Discovery
- `workflows/execute-task/workflow.yaml` - Uses during implementation
- `AGENTS.md` - Primary directive

---

**Remember:** These files are your LEGACY KNOWLEDGE BASE. Use them to be SMART, not DUPLICATE work!

✅ Load what you need
✅ Check before creating
✅ Update after creating
✅ Maintain consistency

🚀 **Result:** Faster development, better quality, no duplication!
