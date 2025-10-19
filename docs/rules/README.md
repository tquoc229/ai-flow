# AI Coding Agent Project Policy Documentation

**Version:** 2.0 (Modular Structure)
**Last Updated:** 2025-10-19

---

## Overview

This directory contains the complete AI Coding Agent Project Policy documentation in a **modular structure** optimized for efficient token usage and easy navigation.

---

## Structure

```
docs/rules/
├── README.md                          # This file
├── project-policy-index.md            # Main entry point (~250 lines)
└── sections/
    ├── 1-fundamentals.md              # Core principles and rules (~350 lines)
    ├── 2-pbi-management.md            # PBI lifecycle workflows (~520 lines)
    ├── 3-task-management.md           # Task workflows and rules (~760 lines)
    ├── 4-testing-strategy.md          # Testing guidelines (~760 lines)
    └── 5-quick-reference.md           # Patterns and checklists (~710 lines)
```

---

## How to Use

### For AI Agents

**Initial Setup:**
1. Read `project-policy-index.md` (main entry point)
2. This gives you complete overview and core principles
3. Load specific sections only when needed

**During Work:**
- Working with PBI? → Load Section 2
- Working with Task? → Load Section 3
- Writing tests? → Load Section 4
- Need checklist? → Load Section 5

**Token Efficiency:**
- Index file: ~250 lines (vs 3000+ in monolithic version)
- Load sections on-demand = 95% token savings

### For Human Users

**First Time:**
1. Start with `project-policy-index.md`
2. Read Section 1 (Fundamentals) completely
3. Skim other sections for overview
4. Deep-dive as needed

**Daily Use:**
- Bookmark Section 5 for quick reference
- Use index for navigation
- Reference specific sections for detailed workflows

---

## Key Changes in Version 2.0

### What's New
- ✅ Split into 5 focused sections plus index
- ✅ Added **Legacy Code Prioritization** principle
- ✅ Added **Hierarchical Planning** (PBI vs Task)
- ✅ Enhanced PBI workflow with Discovery phase
- ✅ New PBI states: `PlanInReview`, `ReadyForTasks`, `NeedsPlanRework`
- ✅ Updated PBI detail document template with Legacy Discovery section
- ✅ Improved navigation with relative path links

### Migration from v1.0
If you have the old monolithic file:
- All content is preserved
- Content reorganized into logical sections
- Navigation improved significantly
- Core workflows unchanged

---

## Section Summaries

### [Index](docs/rules/project-policy-index.md)
**Purpose:** Main navigation hub
**Size:** ~250 lines
**Use for:** Getting started, quick decisions, finding content

**Contains:**
- Core principles summary
- Quick decision guide for AI agents
- Critical rules summary
- Links to all sections
- How to use this documentation

---

### [Section 1: Fundamentals](docs/rules/sections/1-fundamentals.md)
**Purpose:** Core principles governing all work
**Size:** ~350 lines
**Use for:** Understanding system philosophy, automation rules

**Contains:**
- Actors and responsibilities
- Core principles (Task-Driven, DRY, etc.)
- AI Agent automation rules
- PRD alignment requirements
- Change management rules
- Legacy code prioritization
- Hierarchical planning principle

---

### [Section 2: PBI Management](docs/rules/sections/2-pbi-management.md)
**Purpose:** Product Backlog Item workflows
**Size:** ~520 lines
**Use for:** Creating/managing PBIs, understanding PBI lifecycle

**Contains:**
- PBI workflow states (including new states)
- 8 PBI state transitions
- PBI history logging
- Backlog document structure
- PBI detail document template (with Legacy Discovery)
- Bidirectional linking patterns

---

### [Section 3: Task Management](docs/rules/sections/3-task-management.md)
**Purpose:** Task creation and execution workflows
**Size:** ~760 lines
**Use for:** Daily task work, understanding task lifecycle

**Contains:**
- Task workflow states
- 8 task state transitions
- Task status synchronization rules
- Task concurrency limits (one at a time)
- Task history logging
- Version control guidelines
- Task validation rules
- Task index file structure

---

### [Section 4: Testing Strategy](docs/rules/sections/4-testing-strategy.md)
**Purpose:** Testing principles and implementation
**Size:** ~760 lines
**Use for:** Writing test plans, implementing tests

**Contains:**
- Testing principles (Test Pyramid, Risk-based)
- Test scoping by type (Unit, Integration, E2E)
- Test plan proportionality
- Mocking strategies
- Test implementation guidelines
- Best practices with examples

---

### [Section 5: Quick Reference](docs/rules/sections/5-quick-reference.md)
**Purpose:** Practical patterns and checklists
**Size:** ~710 lines
**Use for:** Day-to-day workflows, validation, troubleshooting

**Contains:**
- 6 common workflow patterns
- Git commands and patterns
- 3 decision trees
- 4 validation checklists
- Common pitfalls (5 categories)
- Best practices (4 categories)

---

## Quick Start Examples

### Example 1: AI Agent Starting New Work

```markdown
1. Read: project-policy-index.md (~250 lines)
2. Quick check: "Do I have an agreed task?" (from index)
3. If working on task: Load `docs/rules/sections/3-task-management.md`
4. Follow workflow patterns from `docs/rules/sections/5-quick-reference.md`
```

**Token usage:** ~1200 lines (vs 3000+ for full doc)

### Example 2: Writing Test Plan

```markdown
1. Reference: `project-policy-index.md` (find testing section)
2. Load: `docs/rules/sections/4-testing-strategy.md`
3. Check proportionality matrix for task complexity
4. Follow test plan template
```

**Token usage:** ~1000 lines

### Example 3: Creating New PBI

```markdown
1. Check: project-policy-index.md (PBI workflow summary)
2. Load: docs/rules/sections/2-pbi-management.md
3. Follow Transition 1 and 2 workflows
4. Use PBI detail document template
```

**Token usage:** ~770 lines

---

## File Conventions

### Internal Links
All internal links:
- `[Link text](docs/rules/relative/path/to/file.md)`
- `[Section anchor](docs/rules/file.md#section-name)`

### Navigation Pattern
Each section file has:
- **Header:** Links to Index and Previous section
- **Footer:** Links to Index, Previous, and Next sections

### Markdown Standards
- Use ATX-style headers (`#`, `##`, etc.)
- Code blocks with language identifiers
- Tables for structured data
- Checkboxes for lists `- [ ]` / `- [x]`

---

## Maintenance

### Updating Content
1. Identify which section needs update
2. Edit the specific section file
3. Update version and date in index if major change
4. Verify all links still work

### Adding New Content
1. Determine appropriate section
2. Add content maintaining existing structure
3. Update section summary in index if needed
4. Add to navigation if creating new section

### Version Control
- Use meaningful commit messages
- Reference PBI/Task IDs in commits
- Tag major version changes
- Update version history in index

---

## Support

### Questions?
- Check Section 5 (Quick Reference) first
- Review relevant section for detailed info
- Consult index for navigation

### Found an Issue?
- Document the issue clearly
- Identify affected section(s)
- Propose fix if possible
- Update documentation after fix

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 2.0 | 2025-10-19 | Modular structure, Legacy Discovery, PBI workflow enhancements |
| 1.0 | 2025-10-19 | Initial monolithic version |

---

**For the complete policy, start here:** [project-policy-index.md](docs/rules/project-policy-index.md)
