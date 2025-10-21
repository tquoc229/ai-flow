# Decompose Tasks - Validation Checklist

> **AI Agent**: Use this checklist to validate task decomposition before marking complete.
> Check each item and report results to the user.

---

## 📋 Pre-Decomposition Validation

### Context Loading ✓

- [ ] **AGENTS.md loaded** - Primary directive understood
- [ ] **ai-flow-config.yaml loaded** - Task states and transitions understood
- [ ] **Section 3 (Task Management) loaded** - Task creation rules understood
- [ ] **backlog.md loaded** - PBI exists and status verified
- [ ] **PRD file loaded** - Implementation plan and requirements extracted
- [ ] **All required files readable** - No file access errors

### Variable Collection ✓

- [ ] **pbi_id provided** - User specified PBI ID
- [ ] **pbi_id is numeric** - Valid format
- [ ] **All required variables collected** - No missing inputs

### PBI State Validation ✓

- [ ] **PBI exists in backlog** - PBI ID found
- [ ] **PBI status is ReadyForTasks** - Correct state for decomposition
- [ ] **PRD file exists** - {output_folder}/{pbi_id}/prd.md readable
- [ ] **Implementation Plan section present** - Can extract phases
- [ ] **No policy violations** - PBI state allows decomposition

---

## 🔄 During Decomposition Validation

### Implementation Plan Analysis ✓

- [ ] **All phases identified** - Implementation Plan parsed
- [ ] **Phases are logical** - Correct sequential order
- [ ] **Each phase has deliverables** - Clear outcomes defined
- [ ] **Atomic tasks identified** - Each task independently implementable
- [ ] **Task breakdown appropriate** - Not too granular, not too coarse
- [ ] **Frontend/backend separated** (if applicable) - Clear boundaries
- [ ] **Testing tasks included** (if needed) - Dedicated test tasks

### Task File Creation ✓

- [ ] **PBI directory exists** - {output_folder}/{pbi_id}/
- [ ] **Task files created** - One per task: {pbi_id}-{n}.md
- [ ] **Sequential numbering** - Tasks numbered 1, 2, 3, ...
- [ ] **All required sections present** - Goal, Context, Requirements, etc.
- [ ] **Status History initialized** - Created and Auto-Approved entries
- [ ] **Timestamps generated** - ISO 8601 format
- [ ] **All tasks set to Agreed** - Auto-approval applied

### Task Content Quality ✓

- [ ] **Goal is clear** - Single, specific objective per task
- [ ] **Context meaningful** - Parent PBI context provided
- [ ] **Requirements specific** - Measurable, implementable
- [ ] **Implementation Plan detailed** - Step-by-step instructions
- [ ] **Files Modified listed** - Specific file paths
- [ ] **Verification steps defined** - How to verify completion
- [ ] **Testing requirements included** - Unit, integration tests

### Task Index Creation ✓

- [ ] **tasks.md file created** - {output_folder}/{pbi_id}/tasks.md
- [ ] **Markdown table format** - Proper columns: Task ID, Name, Status, Description
- [ ] **All tasks listed** - Complete index
- [ ] **Links to task files** - Each task ID linked to detail file
- [ ] **Status summary included** - Count of tasks by status
- [ ] **Parent PBI link included** - Link back to prd.md

### Auto-Approval ✓

- [ ] **All tasks set to Agreed** - Not Proposed
- [ ] **Auto-approval logged** - History entry for each task
- [ ] **Justification clear** - "Plan already approved by user"
- [ ] **No tasks skipped** - All tasks auto-approved

---

## ✅ Post-Decomposition Validation

### State Transition: ReadyForTasks → InProgress ✓

- [ ] **Timestamp generated** - ISO 8601 format
- [ ] **Backlog status updated** - ReadyForTasks → InProgress
- [ ] **History entry created** - In PBI History section
- [ ] **PRD frontmatter updated** - status: InProgress
- [ ] **PRD timestamp updated** - updated field set
- [ ] **All transitions logged** - Complete audit trail

### File Structure ✓

- [ ] **PBI directory intact** - No accidental deletions
- [ ] **prd.md preserved** - Original PRD unchanged
- [ ] **tasks.md created** - Task index exists
- [ ] **All task files created** - One per task
- [ ] **No extra files** - Only expected files created
- [ ] **File naming correct** - {pbi_id}-{n}.md format

### Content Integrity ✓

- [ ] **Backlog structure preserved** - No formatting changes to other PBIs
- [ ] **PBI History intact** - Only new entry added
- [ ] **PRD content unchanged** - Only frontmatter updated
- [ ] **Task structure consistent** - All tasks follow same format
- [ ] **No data loss** - All information from PRD captured in tasks

---

## 🔍 Quality Checks

### Task Atomicity ✓

- [ ] **Each task is independent** - Can be implemented standalone
- [ ] **Tasks are focused** - Single responsibility per task
- [ ] **Tasks are completable** - Not open-ended or unbounded
- [ ] **No overlapping scope** - Clear boundaries between tasks
- [ ] **Dependencies identified** - If tasks depend on each other, noted

### Task Completeness ✓

- [ ] **All PRD phases covered** - Every phase has corresponding tasks
- [ ] **All requirements addressed** - Each requirement mapped to task(s)
- [ ] **Testing covered** - Test tasks or test sections included
- [ ] **Documentation included** - Tasks for docs if needed
- [ ] **No gaps** - Nothing from PRD missing in task breakdown

### Task Quality ✓

- [ ] **Goals are actionable** - Clear what to accomplish
- [ ] **Requirements are testable** - Can verify completion
- [ ] **Implementation plans detailed** - Sufficient guidance for AI
- [ ] **Files identified** - Know what to modify/create
- [ ] **Verification clear** - Unambiguous success criteria
- [ ] **Testing adequate** - Appropriate test coverage defined

### Task Sizing ✓

- [ ] **Not too small** - Tasks are meaningful units of work
- [ ] **Not too large** - Tasks are completable in reasonable time
- [ ] **Roughly consistent** - Similar effort/complexity across tasks
- [ ] **Appropriate for complexity** - More complex PBIs → more tasks

---

## 📐 Policy Compliance

### Task Management Policy ✓

- [ ] **Auto-approval applied** - All tasks Agreed, not Proposed
- [ ] **Task structure compliance** - All required sections present
- [ ] **Status History initialized** - Two entries: Created, Auto-Approved
- [ ] **Task index follows format** - Only 4 columns, no extras
- [ ] **Concurrency awareness** - Max 1 task InProgress (currently 0)
- [ ] **History logging complete** - All transitions logged

### Configuration Compliance ✓

- [ ] **States match config** - Using states from ai-flow-config.yaml
- [ ] **Transitions valid** - Following task.transitions from config
- [ ] **Output folder correct** - Using {output_folder} from config
- [ ] **Document language correct** - Using {document_output_language}
- [ ] **Communication language used** - All messages in {communication_language}

### PBI Management Policy ✓

- [ ] **Valid PBI state transition** - ReadyForTasks → InProgress allowed
- [ ] **PBI status updated** - Reflected in backlog.md
- [ ] **PBI history updated** - Transition logged
- [ ] **Bidirectional links maintained** - Tasks → PBI, PBI → Tasks

---

## 📊 Success Criteria (Overall)

### Must-Have ✓

- [ ] **All tasks created** - From Implementation Plan
- [ ] **Each task has detail file** - Complete structure
- [ ] **All tasks Agreed** - Auto-approved
- [ ] **tasks.md index created** - Proper format
- [ ] **PBI status InProgress** - Transitioned correctly
- [ ] **History logged** - All transitions recorded

### Quality ✓

- [ ] **Tasks are atomic** - Independently implementable
- [ ] **Tasks cover all requirements** - Complete coverage
- [ ] **Task breakdown logical** - Phases respected
- [ ] **No duplicated work** - Each task unique
- [ ] **Testing included** - Adequate test coverage

### User Experience ✓

- [ ] **Progress updates shown** - User informed at each step
- [ ] **Task breakdown presented** - User saw proposed tasks
- [ ] **User approved** (or auto-approved) - Confirmation received
- [ ] **Summary provided** - Final report with links
- [ ] **Next steps clear** - User knows how to execute tasks
- [ ] **Communication in correct language** - All messages in {communication_language}

---

## 🎯 Task Decomposition Best Practices

### Task Naming ✓

- [ ] **Names are descriptive** - Clear what task does
- [ ] **Names are concise** - Not overly long
- [ ] **Names follow convention** - Consistent pattern
- [ ] **Names avoid jargon** - Understandable

### Dependency Management ✓

- [ ] **Dependencies identified** - "Depends on" field populated if needed
- [ ] **Dependency order logical** - Can execute in sequence
- [ ] **No circular dependencies** - Acyclic task graph
- [ ] **Blocking tasks noted** - "Blocks" field populated if needed

### Testing Strategy ✓

- [ ] **Test tasks appropriate** - Not every task needs dedicated test task
- [ ] **Testing integrated** - Tests in each task or separate task
- [ ] **Coverage proportional** - Based on task complexity
- [ ] **E2E tests considered** - Included if applicable

---

## 📝 Validation Report Template

Use this template to report validation results to the user:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 TASK DECOMPOSITION VALIDATION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**PBI ID:** {pbi_id}
**Validation Date:** {timestamp}

**Tasks Created:** {task_count}

**Checklist Results:**
- Pre-Decomposition: {passed}/{total} ✓
- During Decomposition: {passed}/{total} ✓
- Post-Decomposition: {passed}/{total} ✓
- Quality Checks: {passed}/{total} ✓
- Policy Compliance: {passed}/{total} ✓

**Overall Pass Rate:** {percentage}%

**Status:** {PASS/FAIL/WARNINGS}

{IF any failures}
**Failed Items:**
- {item_1}: {reason}
- {item_2}: {reason}
...

**Recommended Actions:**
1. {action_1}
2. {action_2}
...
{END IF}

{IF warnings}
**Warnings:**
- {warning_1}
- {warning_2}
...
{END IF}

**Task Breakdown Summary:**
{list all tasks with brief description}

**Conclusion:**
{summary_of_validation_results}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🎯 Pass Criteria

- **Minimum Pass Rate:** 95% overall
- **Critical Items:** ALL must pass (cannot be skipped)
- **Quality Items:** At least 90% must pass
- **Policy Items:** 100% must pass

**Critical Items:**
- All tasks created from Implementation Plan
- Each task has complete detail file
- All tasks set to Agreed
- tasks.md index created
- PBI status updated to InProgress
- History logged for all transitions

If any critical item fails, the task decomposition is considered incomplete.

---

## 💡 Common Issues and Solutions

### Issue: Tasks are too granular
**Solution:** Combine related tasks into single atomic unit

### Issue: Tasks are too large
**Solution:** Break down into smaller, independent tasks

### Issue: Unclear dependencies
**Solution:** Explicitly document in "Depends on" and "Blocks" fields

### Issue: Missing test coverage
**Solution:** Add dedicated test tasks or enhance test sections in tasks

### Issue: Incomplete task descriptions
**Solution:** Review PRD Implementation Plan, extract more details

### Issue: Inconsistent task structure
**Solution:** Use template for all tasks, ensure uniform format

---

## 📚 References

- **Task Management Policy:** {project-root}/docs/rules/sections/3-task-management.md
- **Workflow Configuration:** {project-root}/workflows/decompose-tasks/workflow.yaml
- **Task Template:** Defined in Task Management policy
- **Config File:** {project-root}/ai-flow-config.yaml
