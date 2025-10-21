# Execute Task - AI Instructions

> **AI Agent**: This document is your step-by-step guide for executing a MyFlow task.
> Follow these steps sequentially while exercising your intelligence and judgment.

---

## 🎯 Workflow Objective

Execute a MyFlow task from **Agreed** state to **InReview** state by:
- ✅ Following all MyFlow policies and rules
- ✅ Preserving existing task document structure
- ✅ Implementing all requirements with quality
- ✅ Running tests and validations
- ✅ Documenting all changes thoroughly

**Expected Duration:** 30-120 minutes (varies by task complexity)

---

## 📚 Pre-Execution: Load Context

### Before You Begin

**You MUST read these files first** (in this order):

1. **[execute-task.yaml](./execute-task.yaml)** ← This workflow's configuration
   - Understand variables, rules, expected outputs
   - Check execution settings

2. **[AGENTS.md](../AGENTS.md)** ← Primary directive
   - Your role and responsibilities
   - Critical rules that NEVER can be violated
   - Common mistakes to avoid

3. **[config.yaml](../config.yaml)** ← Central configuration
   - Valid states and transitions
   - Automation settings
   - File path patterns

4. **[docs/rules/project-policy-index.md](../docs/rules/project-policy-index.md)** ← Policy overview
   - Quick decision guide
   - Links to detailed sections

5. **[docs/rules/sections/3-task-management.md](../docs/rules/sections/3-task-management.md)** ← Task workflow rules
   - Task states and transitions
   - Workflow patterns
   - Validation rules

### Ask User for Required Variables

If not already provided, ask user:

```
I need the following information to execute the task:

1. **Task Path**: Full path to the task markdown file
   Example: docs/delivery/14/14-1.md

   → Please provide the task path:

2. **Run Tests**: How should I handle tests?
   - auto (detect and run automatically)
   - manual (I'll prompt you)
   - skip (not recommended)

   → Your preference [auto]:

3. **Auto Commit**: Create git commit after completion?
   - yes
   - no (I'll ask for your approval)

   → Your preference [no]:
```

### Extract Variables

Once you have `task_path`, extract:

```javascript
// Example: docs/delivery/14/14-1.md
const match = task_path.match(/delivery\/(\d+)\/([\d-]+)\.md/);
const pbi_id = match[1];    // "14"
const task_id = match[2];   // "14-1"
```

### Load Task-Specific Context

Now read:

6. **Task file**: `{task_path}`
   - Parse all sections carefully
   - Extract: Description, Requirements, Implementation Plan, Verification, Files Modified

7. **Parent PBI PRD**: `docs/delivery/{pbi_id}/prd.md`
   - Extract: Overview, Technical Approach, Acceptance Criteria, Dependencies

8. **Task Index**: `docs/delivery/{pbi_id}/tasks.md`
   - Check for other tasks in InProgress state (critical for max_concurrent rule)

9. **Testing Strategy** (if task involves tests): `docs/rules/sections/4-testing-strategy.md`

### Display Context Summary

Show user what you've loaded:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 CONTEXT LOADED & READY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Task:** {task_id} - {task_title}
**PBI:** {pbi_id} - {pbi_title}
**Current State:** {current_state}

**Requirements:** ({count} items)
1. {requirement_1}
2. {requirement_2}
...

**Files to Modify:** ({count} files)
- {file_1}
- {file_2}
...

**Test Strategy:** {test_approach}

**Parent PBI Context:**
- Overview: {brief_overview}
- Technical Approach: {brief_tech_approach}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Context loading complete. Ready to validate preconditions.
```

---

## 🔄 Step-by-Step Execution

---

### STEP 1: Validate Preconditions ⚠️

**⚠️ CRITICAL STEP - Do NOT skip any check**

#### Check 1.1: Task State Validation

```
Current task state: {current_state}
Required state: Agreed

IF current_state != "Agreed" THEN
  ❌ ERROR: Task must be in 'Agreed' state to execute

  Current state: {current_state}

  This task needs to be transitioned to 'Agreed' before execution.

  Would you like me to:
  1. Guide you through transitioning to Agreed
  2. Abort this workflow

  → Your choice (1/2):

  IF user chooses abort THEN
    STOP workflow execution
  END IF
END IF
```

#### Check 1.2: Concurrent Tasks Validation (Critical Policy Rule)

```
Policy: Maximum {max_concurrent} task can be InProgress at any time

Reading task index: {pbi_id}/tasks.md
Counting tasks with status = "InProgress"...

Found: {count} task(s) in InProgress state

IF count >= max_concurrent THEN
  ❌ ERROR: Policy violation - max_concurrent limit reached

  The following task(s) are currently InProgress:
  - Task {active_task_id_1}: {active_task_title_1}
  - Task {active_task_id_2}: {active_task_title_2}

  MyFlow policy allows only {max_concurrent} task at a time to ensure focus.

  You must complete or cancel the active task(s) before starting this one.

  HALT: Cannot proceed with active tasks.

  STOP workflow execution
END IF
```

#### Check 1.3: Parent PBI State Validation

```
Reading parent PBI: {pbi_id}/prd.md
Extracting PBI state...

Current PBI state: {pbi_state}
Recommended PBI states for task execution: ReadyForTasks, InProgress

IF pbi_state NOT IN ["ReadyForTasks", "InProgress"] THEN
  ⚠️ WARNING: Parent PBI is in '{pbi_state}' state

  This is unusual. Tasks are typically executed when PBI is in:
  - ReadyForTasks (tasks have been planned and approved)
  - InProgress (actively working on the PBI)

  Current PBI state '{pbi_state}' suggests the PBI may not be ready.

  Do you want to continue anyway? (y/n)

  IF user says no THEN
    STOP workflow execution
  END IF
END IF
```

#### Check 1.4: Display Precondition Summary

```markdown
✅ PRECONDITION VALIDATION COMPLETE

- Task State: Agreed ✓
- Concurrent Tasks: 0 active ✓
- PBI State: {pbi_state} ✓
- All Policies: Compliant ✓
- Required Files: All readable ✓

All preconditions met. Ready to proceed with execution.

Continue to implementation? (y/n)
```

---

### STEP 2: Transition to InProgress 🚀

**Update task state and log the transition**

#### 2.1: Generate Timestamp

```javascript
// Use ISO 8601 format
const timestamp = new Date().toISOString();
// Example: "2025-10-21T14:30:00.000Z"
```

#### 2.2: Create History Entry

```markdown
Preparing history entry:

| {timestamp} | State Transition | Agreed | InProgress | Started execution | {user_name} |

This will be added to the Status History table in the task file.
```

#### 2.3: Update Task File

```
Opening task file for editing: {task_path}

Finding Status History table...
Located at line {line_number}

Inserting new row at the end of table...

Preserving all other content ✓
```

**Use Edit tool to precisely update:**

```
Find the Status History table
Locate the last row
Insert new row with timestamp, state transition details
Verify table formatting is preserved
```

#### 2.4: Update Task Index

```
Opening task index: {pbi_id}/tasks.md

Finding row for task {task_id}...
Current status in table: {current_status_in_table}
Updating to: InProgress

Updated ✓
```

#### 2.5: Update Backlog (if enabled)

```
Checking config: automation.auto_update_backlog = {value}

IF auto_update_backlog == true THEN
  Opening backlog: docs/delivery/backlog.md
  Finding PBI {pbi_id} entry...
  Reflecting task state change...
  Updated ✓
END IF
```

#### 2.6: Confirm Transition

```markdown
✅ Task Transitioned to InProgress

- Status History updated with timestamp
- Task index updated
- Backlog updated (if enabled)
- Current state: InProgress

Ready to begin implementation.
```

---

### STEP 3: Implementation Loop 🔨

**For each requirement in the task, implement with AI guidance**

#### 3.1: Process Each Requirement

```
Total requirements: {count}
Current requirement: {n} of {count}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 REQUIREMENT {n}: {requirement_text}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 3.2: AI Elicitation (Suggest Approaches)

Use your intelligence and loaded context to suggest approaches:

```markdown
💡 IMPLEMENTATION APPROACH SUGGESTIONS

Based on:
- This requirement: "{requirement_text}"
- PBI technical approach: "{tech_approach}"
- Project architecture: "{codebase_structure}"
- Files to modify: {file_list}

**Option 1: {approach_name_1}**
Description: {description_1}
Pros: {pros_1}
Cons: {cons_1}
Complexity: {complexity_1}

**Option 2: {approach_name_2}**
Description: {description_2}
Pros: {pros_2}
Cons: {cons_2}
Complexity: {complexity_2}

**Option 3: {approach_name_3}**
Description: {description_3}
Pros: {pros_3}
Cons: {cons_3}
Complexity: {complexity_3}

**💎 RECOMMENDED: Option {n}**
Rationale: {why_this_is_best}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

How would you like to proceed?
1. Use recommended approach (Option {n})
2. Use alternative (specify 1/2/3)
3. Custom approach (describe it)

→ Your choice:
```

#### 3.3: Implement the Chosen Approach

```
Implementing approach: {chosen_approach}

{Detailed implementation steps based on chosen approach}

Files being modified:
- {file_1}: {changes_1}
- {file_2}: {changes_2}

Implementation complete ✓
```

#### 3.4: Write Tests

```
According to testing strategy, this requirement needs:
- Unit tests: {yes/no}
- Integration tests: {yes/no}

Writing tests...

Created/Modified:
- {test_file_1}: {test_description_1}
- {test_file_2}: {test_description_2}

Tests written ✓
```

#### 3.5: Run Tests (based on run_tests setting)

**If run_tests == "auto":**

```bash
# Detect test command
Reading package.json scripts section...
Detected test command: {command}

Running: {command}

{test_output}

Results:
- Tests run: {count}
- Passed: {passed}
- Failed: {failed}
- Coverage: {coverage}%
```

**If tests fail and strict_mode == true:**

```markdown
❌ TEST FAILURE IN STRICT MODE

{failed_count} test(s) failed:

{detailed_failure_output}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Strict mode is enabled - cannot proceed with failing tests.

Options:
1. Fix the implementation (I'll help debug)
2. Fix the tests (tests might be incorrect)
3. Continue anyway (⚠️ not recommended - violates policy)

→ Your choice (1/2/3):

IF user chooses 3 THEN
  ⚠️ WARNING: Proceeding with failing tests

  This will be documented in implementation notes as technical debt.

  Adding note to task: "Tests failing - requires investigation"
ELSE IF user chooses 1 or 2 THEN
  {Go back and fix the issue, then re-run tests}
END IF
```

**If run_tests == "manual":**

```
Please run the test suite manually:

Recommended command: {detected_command}

After running tests, please report:
- Did all tests pass? (y/n)
- Coverage percentage (if applicable):
- Any failures to document:

→ Test results:
```

**If run_tests == "skip":**

```
⚠️ Skipping test execution per your request

Note: This will be documented in implementation notes.
```

#### 3.6: Document Progress

```
Requirement {n} complete:
- Implementation: ✓
- Tests: ✓ ({status})
- Verification: pending (will do in step 5)

Continue to next requirement? (y/n/pause)
```

#### 3.7: Repeat for All Requirements

```
Requirement {n} of {count} complete.

{IF more requirements}
  Moving to next requirement...
{ELSE}
  All requirements implemented ✓
  Proceeding to verification step.
{END IF}
```

---

### STEP 4: Run Verification Steps ✅

**Execute all verification steps from task file**

#### 4.1: Load Verification Section

```
Reading Verification section from task file...

Found {count} verification items:
1. {verification_1}
2. {verification_2}
3. {verification_3}
...
```

#### 4.2: Execute Each Verification

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧪 VERIFICATION: {verification_item}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

How to verify: {verification_method}

Executing verification...

{verification_steps}

Result: ✅ PASS / ❌ FAIL

{IF FAIL}
  Evidence: {failure_evidence}

  This verification failed. Options:
  1. Fix the implementation
  2. Skip this verification (not recommended)
  3. Document as known issue

  → Your choice:
{END IF}
```

#### 4.3: Verification Summary

```markdown
📊 VERIFICATION RESULTS

✅ {verification_1}: PASS
✅ {verification_2}: PASS
❌ {verification_3}: FAIL - {reason}
✅ {verification_4}: PASS

Overall: {passed}/{total} verifications passed

{IF any failures AND strict_mode}
  ⚠️ Some verifications failed in strict mode

  Please review failures above and decide how to proceed.
{END IF}
```

---

### STEP 5: Complete Test Suite 🧪

**Run full test suite one final time**

#### 5.1: Determine Test Scope

```
Analyzing task complexity and changes made...

Task complexity: {simple/medium/complex}
Code changes: {scope}

Required tests (from testing strategy):
- Unit tests: {required/optional/not_required}
- Integration tests: {required/optional/not_required}
- E2E tests: {required/optional/not_required}
```

#### 5.2: Execute Full Test Suite

```bash
Running complete test suite...

{test_command} --coverage

{full_test_output}
```

#### 5.3: Test Results Summary

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧪 FINAL TEST RESULTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Unit Tests:        ✅ {passed} passed, {failed} failed
Integration Tests: ✅ {passed} passed, {failed} failed
E2E Tests:         {status}

Coverage: {coverage}% ({above/below} target of {target}%)

Total Tests: {total}
Duration: {duration}

{IF all passing}
  ✅ All tests passing - ready for review
{ELSE}
  ❌ {failed_count} test(s) failing

  {IF strict_mode}
    HALT: Cannot mark task as InReview with failing tests

    Action: Marking task as 'Blocked'
    Reason: Test failures in strict mode

    Adding history entry...

    Task has been marked as Blocked. Please fix test failures.

    STOP workflow execution
  {END IF}
{END IF}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### STEP 6: Compile Implementation Notes 📝

**Create comprehensive implementation summary**

#### 6.1: Generate Implementation Summary

```markdown
## Implementation Notes

**Completed:** {ISO_timestamp}
**Implementer:** {user_name}
**Duration:** {duration}

### Summary

{Brief overview of what was implemented}

### Requirements Implemented

1. ✅ {requirement_1}
   - Approach: {approach_used}
   - Files modified: {files}
   - Tests: {test_details}

2. ✅ {requirement_2}
   - Approach: {approach_used}
   - Files modified: {files}
   - Tests: {test_details}

...

### Changes Made

**Files Created:**
- `{file_path}`: {description}
- `{file_path}`: {description}

**Files Modified:**
- `{file_path}`: {description_of_changes}
- `{file_path}`: {description_of_changes}

**Files Deleted:**
- {none / list}

### Tests Added/Modified

**Unit Tests:**
- `{test_file}`: {description}

**Integration Tests:**
- `{test_file}`: {description}

### Test Results

✅ All tests passing
- Unit: {count} tests
- Integration: {count} tests
- Coverage: {percentage}%

### Verification Results

✅ All verification steps passed
- {verification_1}: ✅
- {verification_2}: ✅
- {verification_3}: ✅

### Technical Notes

{Any important technical details, decisions made, trade-offs, etc.}

### Known Issues / Technical Debt

{Any issues discovered, workarounds applied, or technical debt incurred}

{none / list}

### Next Steps

- Ready for code review
- {Any specific review focus areas}

---

**Task is ready for review.**
```

#### 6.2: Append to Task File

```
Opening task file: {task_path}

Scrolling to end of file...

Appending Implementation Notes section...

{Preserve all existing content}
{Add notes at the very end}

Task file updated ✓
```

---

### STEP 7: Transition to InReview 🔍

**Mark task as complete and ready for review**

#### 7.1: Generate Timestamp

```javascript
const timestamp = new Date().toISOString();
```

#### 7.2: Add History Entry

```
Preparing history entry:

| {timestamp} | State Transition | InProgress | InReview | Implementation complete, all tests passing | {user_name} |
```

#### 7.3: Update Task File Status History

```
Opening task file: {task_path}

Finding Status History table...

Appending new row:
{history_entry}

Table updated ✓
```

#### 7.4: Update Task Index

```
Opening task index: {pbi_id}/tasks.md

Finding row for task {task_id}...
Updating status: InProgress → InReview

Updated ✓
```

#### 7.5: Update Backlog

```
{IF auto_update_backlog}
  Updating backlog.md with new task state...
  Updated ✓
{END IF}
```

#### 7.6: Confirm Transition

```markdown
✅ Task Transitioned to InReview

- State: InProgress → InReview
- History: Logged with timestamp
- Task index: Updated
- Backlog: Updated (if enabled)

Task is now ready for code review.
```

---

### STEP 8: Git Commit (Optional) 🔧

**Create git commit if requested**

#### 8.1: Check if Commit Needed

```
Checking auto_commit setting: {auto_commit}

{IF auto_commit == false}
  Would you like to create a git commit for this task? (y/n)

  → Your response:
{END IF}

{IF user declined or skip}
  ℹ️ Skipping git commit

  You can commit manually later using:
  git add {list_of_files}
  git commit -m "feat({pbi_id}): {task_title}"

  SKIP to Step 9
{END IF}
```

#### 8.2: Show Git Status

```bash
Running: git status

{git_status_output}

Files modified:
- {modified_file_1}
- {modified_file_2}
...

Files created:
- {new_file_1}
- {new_file_2}
...
```

#### 8.3: Stage Files

```
Which files should be staged for commit?

1. Stage all modified files listed in task
2. Stage only specific files (you'll select)
3. Cancel commit

→ Your choice:

{IF option 1}
  Staging files:
  {for each file in task's "Files Modified" section}
    git add {file}
  {end for}
{END IF}

{IF option 2}
  {Present list of files, user checks which to stage}
{END IF}
```

#### 8.4: Generate Commit Message

```
Generating commit message...

Format from config: {commit_message_format}

Proposed commit message:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
feat({pbi_id}): {task_title}

{Implementation summary - first 3-5 bullet points}

Task: {task_id}
Tests: ✅ All passing
Files: {file_count} modified
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Approve this commit message? (y/edit/cancel)

→ Your response:
```

#### 8.5: Create Commit

```bash
{IF approved}
  Creating commit...

  git commit -m "{commit_message}"

  Commit created: {commit_hash}

  ✅ Changes committed successfully

  Push to remote? (y/n)

  {IF yes}
    git push

    ✅ Pushed to remote
  {END IF}
{END IF}
```

---

### STEP 9: Validation Checklist ✅

**Run final validation using checklist**

```
Reading validation checklist: execute-task-checklist.md

Running validation items...

{For each item in checklist}
  ☑️ {checklist_item}: {PASS/FAIL}
{End for}

Checklist results:
- Passed: {passed_count}/{total}
- Failed: {failed_count}/{total}

{IF any failures}
  ⚠️ Some validation items failed

  Please review:
  {list failed items}

  These should be addressed before completing task.
{END IF}
```

---

### STEP 10: Final Summary 📊

**Display comprehensive completion summary**

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TASK EXECUTION COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Task:** {task_id} - {task_title}
**PBI:** {pbi_id} - {pbi_title}
**Final State:** InReview
**Duration:** {duration}
**Completed:** {timestamp}

**Implementation Summary:**
- ✅ Requirements implemented: {count}/{total}
- ✅ Files modified: {count}
- ✅ Tests passing: {count}
- ✅ Coverage: {percentage}%
- ✅ Verifications passed: {count}/{total}
- {✅/❌} Git commit created: {yes/no} {commit_hash}

**Files Modified:** ({count} files)
- {file_1}
- {file_2}
- {file_3}
...

**Test Results:**
- Unit tests: ✅ {count} passing
- Integration tests: ✅ {count} passing
- Total: {total} tests
- Coverage: {percentage}%

**State Transitions:**
1. Agreed → InProgress ({timestamp_1})
2. InProgress → InReview ({timestamp_2})

**Documentation:**
- Task file updated with implementation notes
- Status history logged
- Task index updated
- Backlog updated (if enabled)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📋 Next Steps:**

1. **Request Code Review** from team member
   - Focus areas: {specific_areas_if_any}

2. **Address Review Comments** (if any)
   - Make requested changes
   - Re-run tests

3. **Transition to Testing** (after review approval)
   - Run acceptance tests
   - Verify in staging environment

4. **Mark as Done** when fully tested and accepted

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📂 Quick Links:**

- Task file: [{task_id}.md]({task_path})
- PBI: [PBI {pbi_id}]({pbi_path})
- Task index: [tasks.md]({tasks_index_path})
- Backlog: [backlog.md]({backlog_path})
{IF commit created}
- Commit: {commit_hash}
{END IF}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Workflow execution completed successfully! 🎉**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🚨 Error Handling Guide

### Precondition Failures

**Task Not in Agreed State:**
```
HALT immediately
Display current state
Explain state must be Agreed
Offer to guide transition
Do NOT proceed
```

**Concurrent Task Violation:**
```
HALT immediately
List active InProgress tasks
Explain max_concurrent: 1 policy
Suggest completing or canceling active tasks
Do NOT proceed
```

**Parent PBI Wrong State:**
```
WARN user
Explain typical PBI states
Ask if they want to continue
Respect user decision
```

### Execution Failures

**Tests Fail in Strict Mode:**
```
Mark task as Blocked
Document test failures
Add history entry with details
HALT workflow
Offer suggestions for fixing
Do NOT mark as InReview
```

**Verification Fails:**
```
Document failure
Ask user how to proceed:
  1. Fix implementation
  2. Skip verification (warn)
  3. Document as known issue
Respect user choice
```

### User Cancellation

**Mid-Execution Cancel:**
```
Keep task in InProgress state
Document progress so far
Note: "Execution paused - can be resumed"
Save all work completed
Allow clean resume later
```

---

## 💡 Tips for AI Execution

### General Principles

1. **Always load context first** - Don't make assumptions
2. **Follow steps sequentially** - But use judgment for edge cases
3. **Ask at decision points** - Don't guess user intent
4. **Preserve structure** - NEVER reformat existing documents
5. **Document thoroughly** - Implementation notes are crucial
6. **Be transparent** - Show what you're doing and why
7. **Validate early** - Catch issues in preconditions, not at end
8. **Test rigorously** - Tests are not optional
9. **Respect policies** - Halt on violations in strict mode
10. **Communicate clearly** - Use formatting for readability

### When to HALT

HALT immediately on:
- ❌ Task not in Agreed state
- ❌ Concurrent task violation (max_concurrent)
- ❌ Test failures in strict mode
- ❌ Policy violations
- ❌ User explicitly cancels

### When to WARN (but continue if user approves)

WARN but allow override on:
- ⚠️ Parent PBI in unusual state
- ⚠️ Verification failures (non-critical)
- ⚠️ Test coverage below target (but tests passing)
- ⚠️ Skipping tests (user choice)

### Adaptive Intelligence

You're an AI - use your intelligence:
- **Suggest better approaches** if you see them
- **Catch logical errors** in requirements
- **Recommend improvements** to code quality
- **Detect patterns** from project architecture
- **Adapt wording** to user's communication style
- **Provide context** from related code
- **Anticipate issues** before they occur

---

## ✅ Success Criteria Checklist

Before marking task complete, verify:

- [ ] Task transitioned: Agreed → InProgress → InReview
- [ ] All requirements from task file implemented
- [ ] All files in "Files Modified" section actually modified
- [ ] Tests written for new functionality
- [ ] All tests passing (or failures documented)
- [ ] All verification steps executed and passed
- [ ] Implementation notes appended to task file
- [ ] Status History table updated twice (with timestamps)
- [ ] Task index updated to InReview
- [ ] Backlog updated (if auto_update enabled)
- [ ] No other tasks in InProgress state
- [ ] All changes documented clearly
- [ ] Git commit created (if requested)
- [ ] Validation checklist run and passed
- [ ] User satisfied with implementation

---

**End of Instructions**

**Remember:** You are an intelligent AI agent. These instructions are a guide, not rigid commands. Exercise judgment, adapt to context, and always prioritize quality and user intent. Good luck! 🚀