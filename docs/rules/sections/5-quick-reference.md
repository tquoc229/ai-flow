# Section 5: Quick Reference

[← Back to Index](mdc:../project-policy-index.md)

---

## 📚 Quick Reference Guide

### Common Workflow Patterns

#### Pattern 1: Starting a New PBI

**Step-by-Step:**

```
1. Create PBI entry in backlog.md
   ├─ Assign unique ID (sequential)
   ├─ Write user story: "As a [actor], I want [goal], so that [benefit]"
   ├─ Define Conditions of Satisfaction
   └─ Set status to Proposed

2. User reviews and approves
   └─ Status: Proposed → Agreed

3. AI_Agent creates PBI structure
   ├─ Create: docs/delivery/<PBI-ID>/prd.md
   ├─ Create: docs/delivery/<PBI-ID>/tasks.md
   ├─ Populate all required sections
   ├─ Set up bidirectional linking
   └─ Log approval in PBI history

4. Define tasks in tasks.md
   ├─ Break down PBI into atomic tasks
   ├─ Create task file for each: <PBI-ID>-<TASK-ID>.md
   ├─ Link tasks in task index
   └─ All tasks start as Proposed

5. Ready to start implementation
   └─ PBI status: Agreed → InProgress
```

---

#### Pattern 2: Working on a Task

**Complete Workflow:**

```
Step 1: Pre-Work Validation
├─ Check task exists in both task file AND index
├─ Verify status is "Agreed" in both locations
├─ Confirm no other tasks are "InProgress" for same PBI
└─ Review implementation plan is clear

Step 2: Start Work (Agreed → InProgress)
├─ AI_Agent updates status to "InProgress" in BOTH locations
├─ Log start event in task history with timestamp
├─ Create feature branch (if using version control)
└─ Begin implementing according to plan

Step 3: During Implementation
├─ Follow the implementation plan step by step
├─ Only make changes within task scope
├─ Run tests continuously as you code
├─ Document all files you modify
└─ Include task ID in every commit message

Step 4: Submit for Review (InProgress → Review)
├─ Verify all requirements are implemented
├─ Run complete test suite - all must pass
├─ Update task file with implementation details
├─ List all modified files in task documentation
├─ Update status to "Review" in BOTH locations
├─ Notify User that task is ready
└─ Log submission in task history

Step 5: User Reviews Task
├─ If User Approves (Review → Done):
│  ├─ Review next tasks - are they still relevant?
│  ├─ Confirm with User about next tasks
│  ├─ Update status to "Done" in BOTH locations
│  ├─ Run: git acp "<task-id> <description>"
│  └─ Log completion with timestamp
│
└─ If User Rejects (Review → InProgress):
   ├─ Document specific rejection reasons
   ├─ Update task with User's feedback
   ├─ Update status to "InProgress" in BOTH locations
   └─ Resume work to fix issues
```

**Key Points to Remember:**
- ✅ ALWAYS update both task file AND index together
- ✅ NEVER have two tasks InProgress for same PBI
- ✅ ALWAYS log status changes with timestamp
- ✅ ALWAYS reference task ID in commits

---

#### Pattern 3: Handling Blocked Tasks

**Complete Workflow:**

```
Step 1: Task Gets Blocked
├─ Identify what is preventing progress:
│  ├─ External dependency not available?
│  ├─ Waiting for User decision?
│  ├─ Technical issue can't be resolved?
│  └─ Missing critical information?
└─ Stop all work immediately

Step 2: Mark Task as Blocked (InProgress → Blocked)
├─ Document the specific blocking reason clearly:
│  ├─ What exactly is blocking?
│  ├─ What dependency is missing?
│  ├─ What decision is needed from User?
│  └─ What information is required?
├─ Update status to "Blocked" in BOTH locations
├─ Log blocking event in task history
└─ Notify User immediately with blocking details

Step 3: While Task is Blocked
├─ Monitor the blocker for resolution
├─ Keep User updated on any changes
├─ Create new tasks to address blocker (if you can)
└─ Do NOT work on this task until unblocked

Step 4: Blocker Gets Resolved
├─ Verify the blocker is fully resolved
├─ Dependency is now available OR
├─ User has made the decision OR
└─ Technical issue is fixed

Step 5: Unblock Task (Blocked → InProgress)
├─ Document how blocker was resolved
├─ Update task file with resolution details
├─ Update status to "InProgress" in BOTH locations
├─ Log unblocking event in task history
├─ Resume work from where you stopped
└─ Notify User and stakeholders work resumed
```

**Common Blocking Reasons:**
- 🔴 Waiting for external API credentials
- 🔴 User needs to make architectural decision
- 🔴 Dependency on another team's work
- 🔴 Required third-party service unavailable
- 🔴 Missing design specifications
- 🔴 Technical limitation discovered

**What to Do While Blocked:**
- ✅ Document everything about the blocker
- ✅ Propose solutions if possible
- ✅ Work on other non-blocked tasks (different PBI)
- ❌ Do NOT try to work around the blocker
- ❌ Do NOT start other tasks in same PBI

---

#### Pattern 4: Creating Task Documentation

**Task File Template:**

```markdown
# <Task-ID> <Task-Name>

[Back to task list](mdc:tasks.md)

## Description

[Clear description of what needs to be accomplished]
[Include context, motivation, and expected outcome]

## Status History

| Timestamp | Event Type | From Status | To Status | Details | User |
|-----------|------------|-------------|-----------|---------|------|
| YYYY-MM-DD HH:MM:SS | Created | N/A | Proposed | Initial creation | username |
| YYYY-MM-DD HH:MM:SS | Approved | Proposed | Agreed | Task approved and analysis complete | username |

## Requirements

- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

## Implementation Plan

### Step 1: [First step]
[Details...]

### Step 2: [Second step]
[Details...]

### Step 3: [Final step]
[Details...]

## Test Plan

[Proportional to task complexity - see Section 5.4]

### Objective
[What are we verifying?]

### Test Scenarios
1. Scenario 1
2. Scenario 2

### Success Criteria
- ✅ Criterion 1
- ✅ Criterion 2

## Verification

[How to verify the implementation works correctly]

**Steps:**
1. Step 1
2. Step 2
3. Expected result

## Files Modified

- `path/to/file1.ts`
- `path/to/file2.ts`
- `path/to/file3.test.ts`
```

---

#### Pattern 5: Status Synchronization Check

**Before ANY Status Change:**

```javascript
// Pseudo-code for validation
function validateStatusChange(taskId, newStatus) {
  // 1. Read status from task file
  const fileStatus = readTaskFile(taskId).status;

  // 2. Read status from index
  const indexStatus = readTaskIndex(taskId).status;

  // 3. Verify they match
  if (fileStatus !== indexStatus) {
    throw new Error(
      `Status mismatch for ${taskId}:
       File: ${fileStatus}, Index: ${indexStatus}`
    );
  }

  // 4. Verify transition is valid
  if (!isValidTransition(fileStatus, newStatus)) {
    throw new Error(
      `Invalid transition: ${fileStatus} → ${newStatus}`
    );
  }

  // 5. Update both atomically
  updateTaskFile(taskId, newStatus);
  updateTaskIndex(taskId, newStatus);
  logHistoryEntry(taskId, fileStatus, newStatus);
}
```

---

#### Pattern 6: External Package Integration

**When Using External Package:**

```
1. Research First
   ├─ Search official documentation
   ├─ Understand API usage
   ├─ Check compatibility
   └─ Review examples

2. Create Package Guide
   ├─ File: tasks/<task-id>-<package-name>-guide.md
   ├─ Date stamp the document
   ├─ Link to original docs
   ├─ Include API usage examples
   ├─ Add code snippets in project language
   └─ Document gotchas and best practices

3. Reference in Task
   ├─ Link package guide from task file
   ├─ Use guide as implementation reference
   └─ Update guide if API changes

Example:
Task 2-1 using pg-boss
├─ Create: tasks/2-1-pg-boss-guide.md
├─ Document pg-boss API usage
└─ Reference in task 2-1.md
```

---

### Common Commands and Patterns

#### Git Workflow for Task Completion

```bash
# When task moves to Done status

# Option 1: Using automation helper
git acp "1-7 Add pino logging to help debug database connection issues"

# Option 2: Manual steps
git add .
git commit -m "1-7 Add pino logging to help debug database connection issues"
git push origin feature/1-7-add-pino-logging
```

#### Creating New PBI Structure

```bash
# After PBI approved (Proposed → Agreed)

# Create directory
mkdir -p docs/delivery/<PBI-ID>

# Create files
touch docs/delivery/<PBI-ID>/prd.md
touch docs/delivery/<PBI-ID>/tasks.md

# Populate templates
# - prd.md: Use PBI detail template (Section 3.6)
# - tasks.md: Use task index template (Section 4.10)
```

#### Checking Task Status

```bash
# Check for status mismatches

# 1. Read task file status
grep "Status" docs/delivery/<PBI-ID>/<PBI-ID>-<TASK-ID>.md

# 2. Read index status
grep "<TASK-ID>" docs/delivery/<PBI-ID>/tasks.md

# 3. Compare - they must match!
```

---

### Decision Trees

#### Decision: Should I Search for Package Documentation?

```
Are you about to use an external package?
    ↓
  YES → Do you fully understand its API?
    ↓              ↓
   NO             YES
    ↓              ↓
Search web     Verify your
for official   understanding
docs first     is current
    ↓              ↓
Create package  Proceed with
guide document  implementation
```

---

#### Decision: What Test Plan Detail Level?

```
Assess task complexity
    ↓
Simple? (constants, interfaces, config)
    ↓
  YES → Minimal test plan
    |   - Objective
    |   - Success criteria
    ↓
   NO
    ↓
Basic? (simple functions, CRUD)
    ↓
  YES → Moderate test plan
    |   - Objective
    |   - Test scope
    |   - 3-5 scenarios
    |   - Success criteria
    ↓
   NO
    ↓
Complex? (multi-service, complex logic)
    ↓
  YES → Detailed test plan
        - Objective
        - Test scope
        - Environment & setup
        - Mocking strategy
        - 5-10 scenarios
        - Edge cases
        - Success criteria
```

---

#### Decision: Can I Start This Task?

**Step-by-Step Decision Process:**

```
Question 1: Is the task status "Agreed"?
    ↓
   NO → ❌ STOP. You cannot start this task.
    |   → The task needs User approval first.
    |   → Wait for User to review and approve.
    ↓
  YES → Continue to next check
    ↓

Question 2: Does the status match in BOTH locations?
    ↓
    Check task file status: _______
    Check index file status: _______
    ↓
   NO → ❌ STOP. Status mismatch detected!
    |   → Report this issue to User immediately.
    |   → Provide details: which file has which status.
    |   → Wait for User to fix synchronization.
    ↓
  YES → Continue to next check
    ↓

Question 3: Are there any other tasks "InProgress" for this PBI?
    ↓
  YES → ❌ STOP. Cannot start this task.
    |   → Only ONE task per PBI can be InProgress.
    |   → Report which other task is InProgress.
    |   → Wait for that task to complete first.
    ↓
   NO → Continue to next check
    ↓

Question 4: Are all dependencies available?
    ↓
   NO → ❌ STOP. Task is blocked.
    |   → Mark task as Blocked.
    |   → Document what dependencies are missing.
    |   → Notify User about the blocker.
    ↓
  YES → ✅ All checks passed!
    ↓

    ✅ You can start this task!
    ↓
    Proceed with "Starting Work on Task" transition:
    1. Update status to "InProgress" in BOTH locations
    2. Log start event in task history
    3. Begin implementation
```

**Quick Checklist:**
- [ ] Task status is "Agreed" ✅
- [ ] Status matches in task file and index ✅
- [ ] No other tasks "InProgress" for this PBI ✅
- [ ] All dependencies available ✅

**If ALL checked → Start work!**

---

### Validation Checklists

#### Before Starting Any Task

- [ ] Task exists in task file (`<PBI-ID>-<TASK-ID>.md`)
- [ ] Task exists in task index (`tasks.md`)
- [ ] Status is `Agreed` in task file
- [ ] Status is `Agreed` in task index
- [ ] No other tasks `InProgress` for this PBI
- [ ] All dependencies are available
- [ ] Implementation plan is clear
- [ ] Test plan is defined

---

#### Before Submitting Task for Review

- [ ] All requirements implemented
- [ ] All tests written and passing
- [ ] Test plan requirements met
- [ ] Code follows project standards
- [ ] Files modified documented in task file
- [ ] Task documentation updated with implementation details
- [ ] No scope creep (all changes within task scope)
- [ ] Commits reference task ID

---

#### Before Marking Task as Done

- [ ] User has reviewed and approved
- [ ] All acceptance criteria verified
- [ ] All tests passing
- [ ] Status updated in BOTH locations
- [ ] Next tasks reviewed for relevance
- [ ] User confirmed about next tasks
- [ ] Version control workflow executed
- [ ] Completion logged in history

---

#### Before Marking PBI as Done

- [ ] ALL tasks have status `Done`
- [ ] All Conditions of Satisfaction met
- [ ] Full test suite passing
- [ ] Documentation updated
- [ ] E2E CoS test completed and passing
- [ ] User approval received
- [ ] Completion date recorded
- [ ] History logged

---

## ⚠️ Common Pitfalls to Avoid

### File Management Pitfalls

❌ **Creating files without User confirmation**
- Always get explicit approval for new standalone files
- Only create files within defined structures (PBI, task, source code)

❌ **Orphaned task files**
- Every task in index MUST have corresponding markdown file
- Create task file IMMEDIATELY when adding to index

❌ **Broken links**
- Always use `mdc:` protocol for internal links
- Test links after creation
- Maintain bidirectional linking

---

### Status Management Pitfalls

❌ **Updating only one location**
- ALWAYS update both task file AND index atomically
- Use same commit for both updates

❌ **Skipping status history**
- ALWAYS log status changes with timestamp and user
- Include meaningful details about the change

❌ **Invalid transitions**
- Follow defined workflow states
- Verify transition is allowed before executing

---

### Workflow Pitfalls

❌ **Multiple InProgress tasks**
- Only ONE task per PBI should be InProgress
- Get User approval for exceptions

❌ **Scope creep**
- Stay within defined task scope
- Create new tasks for additional work
- Roll back unauthorized changes

❌ **Missing User approval**
- Never start work on `Proposed` tasks
- Wait for explicit User approval

---

### Testing Pitfalls

❌ **Over-engineering simple test plans**
- Match test plan detail to task complexity
- Simple tasks need simple test plans

❌ **Skipping E2E CoS test task**
- Every PBI MUST have dedicated E2E testing task
- Don't mark PBI done without E2E tests

❌ **Testing package APIs directly**
- Don't test external package methods in unit tests
- Packages have their own tests

---

### Documentation Pitfalls

❌ **Duplicating information**
- Follow DRY principle
- Reference instead of duplicating
- Exception: Titles/names for clarity

❌ **Using magic numbers**
- Define constants for repeated values
- Use constants for special values
- Name constants descriptively

❌ **Missing package guides**
- Always create guide when using new package
- Research official docs first
- Date-stamp and link to sources

---

## ✅ Best Practices Summary

### Process Best Practices

1. **Always verify before acting**
   - Check task status in both locations
   - Validate transitions are allowed
   - Confirm no conflicts

2. **Keep tasks small and focused**
   - Break down complex features
   - One cohesive unit of work per task
   - Easier to review and test

3. **Document as you go**
   - Update files modified in real-time
   - Log history immediately
   - Keep documentation current

4. **Synchronize atomically**
   - Update both locations in same commit
   - Never update one without the other
   - Verify synchronization after updates

---

### Code Best Practices

1. **Use constants for values**
   - Define once, reference everywhere
   - Name constants descriptively
   - Apply to all repeated values

2. **Follow DRY principle**
   - Single source of truth
   - Reference instead of duplicate
   - Reduce inconsistency risk

3. **Reference task ID in commits**
   - Format: `<task-id> <description>`
   - Every commit traceable to task
   - Clear audit trail

4. **Stay within scope**
   - Only changes authorized by task
   - Identify scope creep early
   - Create new tasks for extras

---

### Testing Best Practices

1. **Match test plan to complexity**
   - Simple tasks → minimal plans
   - Complex tasks → detailed plans
   - Don't over-engineer

2. **Use real infrastructure for integration**
   - Test database instances
   - Real message queues
   - Mock only external services

3. **Focus E2E on critical paths**
   - Reserve for important workflows
   - Don't test everything E2E
   - Keep E2E tests minimal

4. **Automate all tests**
   - Consistent verification
   - Fast feedback
   - Regression prevention

---

### Communication Best Practices

1. **Ask when uncertain**
   - Never assume User intent
   - Clarify ambiguous requirements
   - Confirm before significant changes

2. **Report issues immediately**
   - Don't hide problems
   - Stop work when blocked
   - Provide clear error descriptions

3. **Review next tasks**
   - Before marking current task Done
   - Confirm relevance with User
   - Adapt plan as needed

4. **Keep stakeholders informed**
   - Notify on status changes
   - Report blockers
   - Share completion updates

---

**End of Document**

---

[← Back to Index](mdc:../project-policy-index.md)
