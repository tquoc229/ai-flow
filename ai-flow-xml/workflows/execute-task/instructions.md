# Execute Task - Workflow Instructions

<critical>The workflow execution engine is governed by: {project-root}/ai-flow-config.yaml</critical>
<critical>You MUST have already loaded and processed: {project-root}/workflows/execute-task/workflow.yaml</critical>
<critical>Communicate in {communication_language} throughout task execution</critical>
<critical>Address user as {user_name} in greetings and summaries</critical>

<workflow>

<step n="0" goal="Load context and validate workflow.yaml">
<action>Verify that workflow.yaml has been loaded and processed</action>
<action>Extract standard config variables: {user_name}, {communication_language}, {output_folder}, {date}</action>
<check>IF workflow.yaml not loaded THEN HALT with error message</check>
<action>Greet {user_name} in {communication_language}</action>
</step>

<step n="1" goal="Collect required variables from user">
<ask>What is the path to the task file you want to execute?
Example: docs/delivery/14/14-1.md

→ Task path:</ask>

<action>Store user response as {task_path}</action>
<action>Extract {pbi_id} and {task_id} from path using pattern: delivery/{pbi_id}/{task_id}.md</action>

<ask>How should I handle test execution?
1. auto - Auto-detect and run tests automatically
2. manual - I'll prompt you to run tests
3. skip - Skip tests (not recommended)

→ Your choice [auto]:</ask>

<action>Store as {run_tests} (default: "auto")</action>

<ask>Should I create a git commit after completion?
- yes - Create commit automatically
- no - Ask for approval before committing

→ Your choice [no]:</ask>

<action>Store as {auto_commit} (default: false)</action>

<template-output>user_variables</template-output>
</step>

<step n="2" goal="Load all required context files">
<action>Load the following files in order:</action>

<action>1. Primary directive: {project-root}/AGENTS.md</action>
<action>2. Configuration: {project-root}/ai-flow-config.yaml</action>
<action>3. Policy index: {project-root}/docs/rules/project-policy-index.md</action>
<action>4. Task management rules: {project-root}/docs/rules/sections/3-task-management.md</action>
<action>5. Task file: {task_path}</action>
<action>6. Parent PBI: {output_folder}/{pbi_id}/prd.md</action>
<action>7. Task index: {output_folder}/{pbi_id}/tasks.md</action>

<action if="task involves tests">Load testing strategy: {project-root}/docs/rules/sections/4-testing-strategy.md</action>

<action>Parse task file and extract:
- Task title
- Current status (from Status History table - last entry)
- Requirements (all items from Requirements section)
- Files to modify (from Files Modified section)
- Verification steps (from Verification section)
- Implementation plan (from Implementation Plan section)
</action>

<action>Parse PBI file and extract:
- PBI title
- Overview
- Technical Approach
- Acceptance Criteria
</action>

<template-output>loaded_context</template-output>
</step>

<step n="3" goal="Display context summary to user">
<action>Show comprehensive context summary in {communication_language}:</action>

<example>
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
</example>

<template-output>context_summary</template-output>
</step>

<step n="4" goal="Validate preconditions - CRITICAL">
<critical>This step MUST NOT be skipped - validates all policy requirements</critical>

<action>Check 4.1: Task State Validation</action>
<check>Read current task state from Status History table (last entry)</check>
<check>IF current_state != "Agreed" THEN
  Display error in {communication_language}:
  "❌ ERROR: Task must be in 'Agreed' state to execute

  Current state: {current_state}

  This task needs to be transitioned to 'Agreed' before execution.

  Would you like me to:
  1. Guide you through transitioning to Agreed
  2. Abort this workflow"

  IF user chooses abort THEN HALT workflow execution
END IF</check>

<action>Check 4.2: Concurrent Tasks Validation (max_concurrent: 1 rule)</action>
<check>Read task index: {output_folder}/{pbi_id}/tasks.md</check>
<check>Count tasks with status = "InProgress"</check>
<check>IF count >= 1 THEN
  Display error in {communication_language}:
  "❌ ERROR: Policy violation - max_concurrent limit reached

  The following task(s) are currently InProgress:
  - Task {active_task_id}: {active_task_title}

  MyFlow policy allows only 1 task at a time to ensure focus.

  You must complete or cancel the active task(s) before starting this one.

  HALT: Cannot proceed with active tasks."

  HALT workflow execution
END IF</check>

<action>Check 4.3: Parent PBI State Validation</action>
<check>Read parent PBI state from {output_folder}/{pbi_id}/prd.md</check>
<check>IF pbi_state NOT IN ["ReadyForTasks", "InProgress"] THEN
  Display warning in {communication_language}:
  "⚠️ WARNING: Parent PBI is in '{pbi_state}' state

  This is unusual. Tasks are typically executed when PBI is in:
  - ReadyForTasks (tasks have been planned and approved)
  - InProgress (actively working on the PBI)

  Current PBI state '{pbi_state}' suggests the PBI may not be ready.

  Do you want to continue anyway? (y/n)"

  IF user says no THEN HALT workflow execution
END IF</check>

<action>Display precondition summary in {communication_language}:</action>
<example>
✅ PRECONDITION VALIDATION COMPLETE

- Task State: Agreed ✓
- Concurrent Tasks: 0 active ✓
- PBI State: {pbi_state} ✓
- All Policies: Compliant ✓
- Required Files: All readable ✓

All preconditions met. Ready to proceed with execution.

Continue to implementation? (y/n)
</example>

<template-output>precondition_status</template-output>
</step>

<step n="5" goal="Transition task to InProgress">
<action>Generate ISO 8601 timestamp using {date} variable</action>
<action>Create history entry: "| {timestamp} | State Transition | Agreed | InProgress | Started execution | {user_name} |"</action>
<action>Update {task_path} - find Status History table and append new row</action>
<action>Update {output_folder}/{pbi_id}/tasks.md - change status from Agreed to InProgress</action>
<action if="config: automation.auto_update_backlog == true">Update {output_folder}/backlog.md with task state change</action>

<action>Confirm transition in {communication_language}:</action>
<example>
✅ Task Transitioned to InProgress

- Status History updated with timestamp
- Task index updated
- Backlog updated (if enabled)
- Current state: InProgress

Ready to begin implementation.
</example>

<template-output>transition_to_inprogress</template-output>
</step>

<step n="6" goal="Implementation loop - process each requirement" repeat="for-each-requirement">
<action>For requirement {n} of {total_requirements}:</action>

<action>Display requirement header in {communication_language}:</action>
<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 REQUIREMENT {n}: {requirement_text}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<action>AI Elicitation - Suggest 3 implementation approaches based on:
- This requirement: "{requirement_text}"
- PBI technical approach: "{tech_approach}"
- Project architecture: "{codebase_structure}"
- Files to modify: {file_list}
</action>

<action>Present approaches to {user_name} in {communication_language}:</action>
<example>
💡 IMPLEMENTATION APPROACH SUGGESTIONS

Based on the loaded context, I suggest:

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
</example>

<action>Implement the chosen approach</action>
<action>Write tests according to testing strategy (if applicable)</action>

<action if="run_tests == 'auto'">
  Run test command (auto-detected from package.json)
  Display test results

  IF tests fail AND strict_mode == true THEN
    Display error in {communication_language}:
    "❌ TEST FAILURE IN STRICT MODE

    {failed_count} test(s) failed:
    {detailed_failure_output}

    Strict mode is enabled - cannot proceed with failing tests.

    Options:
    1. Fix the implementation (I'll help debug)
    2. Fix the tests (tests might be incorrect)
    3. Continue anyway (⚠️ not recommended - violates policy)

    → Your choice (1/2/3):"

    IF user chooses 3 THEN
      Add note: "Tests failing - requires investigation"
    ELSE IF user chooses 1 or 2 THEN
      Go back and fix, then re-run tests
    END IF
  END IF
</action>

<action if="run_tests == 'manual'">
  Ask {user_name} in {communication_language}:
  "Please run the test suite manually:

  Recommended command: {detected_command}

  After running tests, please report:
  - Did all tests pass? (y/n)
  - Coverage percentage (if applicable):
  - Any failures to document:

  → Test results:"
</action>

<action>Document progress:
"Requirement {n} complete:
- Implementation: ✓
- Tests: ✓ ({status})
- Verification: pending (will do in step 7)

Continue to next requirement? (y/n/pause)"
</action>

<template-output>requirement_{n}_implementation</template-output>
</step>

<step n="7" goal="Execute verification steps">
<action>Load verification items from task file Verification section</action>
<action>For each verification item, execute and document result</action>

<example>
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
</example>

<action>Display verification summary in {communication_language}:</action>
<example>
📊 VERIFICATION RESULTS

✅ {verification_1}: PASS
✅ {verification_2}: PASS
❌ {verification_3}: FAIL - {reason}
✅ {verification_4}: PASS

Overall: {passed}/{total} verifications passed
</example>

<template-output>verification_results</template-output>
</step>

<step n="8" goal="Run complete test suite - final validation">
<action>Determine test scope based on task complexity from testing strategy</action>

<action if="run_tests == 'auto'">
  Run complete test suite with coverage:
  {test_command} --coverage

  Display results in {communication_language}
</action>

<example>
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

    HALT workflow execution
  {END IF}
{END IF}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<template-output>final_test_results</template-output>
</step>

<step n="9" goal="Compile implementation notes">
<action>Generate comprehensive implementation summary including:
- Completion timestamp
- Implementer: {user_name}
- Duration
- Requirements implemented
- Changes made (files created/modified/deleted)
- Tests added/modified
- Test results
- Verification results
- Technical notes
- Known issues / technical debt
- Next steps
</action>

<action>Append Implementation Notes section to {task_path} at the very end</action>
<action>Preserve all existing content - only append new section</action>

<template-output>implementation_notes</template-output>
</step>

<step n="10" goal="Transition task to InReview">
<action>Generate ISO 8601 timestamp using {date}</action>
<action>Create history entry: "| {timestamp} | State Transition | InProgress | InReview | Implementation complete, all tests passing | {user_name} |"</action>
<action>Update {task_path} - append new row to Status History table</action>
<action>Update {output_folder}/{pbi_id}/tasks.md - change status from InProgress to InReview</action>
<action if="config: automation.auto_update_backlog == true">Update {output_folder}/backlog.md</action>

<action>Confirm transition in {communication_language}:</action>
<example>
✅ Task Transitioned to InReview

- State: InProgress → InReview
- History: Logged with timestamp
- Task index: Updated
- Backlog: Updated (if enabled)

Task is now ready for code review.
</example>

<template-output>transition_to_inreview</template-output>
</step>

<step n="11" goal="Git commit (optional)" optional="true">
<check>IF auto_commit == false THEN
  Ask {user_name} in {communication_language}:
  "Would you like to create a git commit for this task? (y/n)

  → Your response:"

  IF user declined THEN
    Display:
    "ℹ️ Skipping git commit

    You can commit manually later using:
    git add {list_of_files}
    git commit -m 'feat({pbi_id}): {task_title}'"

    SKIP to Step 12
  END IF
END IF</check>

<action>Show git status</action>
<action>Ask which files to stage (all from task OR specific selection)</action>
<action>Generate commit message using format from config: "feat({pbi_id}): {task_title}"</action>
<action>Present commit message for approval</action>
<action if="approved">Create commit with git commit -m "{commit_message}"</action>
<action if="commit created">Ask: "Push to remote? (y/n)"</action>
<action if="yes">Run git push</action>

<template-output>git_commit_status</template-output>
</step>

<step n="12" goal="Run validation checklist">
<action>Load validation checklist from: {project-root}/workflows/execute-task-checklist/workflow.md</action>
<action>Execute each validation item and mark PASS/FAIL</action>
<action>Calculate pass rate: {passed}/{total} items</action>

<check>IF pass_rate < 95% THEN
  Display warning in {communication_language}:
  "⚠️ Some validation items failed

  Please review:
  {list failed items}

  These should be addressed before completing task."
END IF</check>

<template-output>validation_checklist_results</template-output>
</step>

<step n="13" goal="Display final summary">
<action>Show comprehensive completion summary to {user_name} in {communication_language}:</action>

<example>
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
</example>

<template-output>final_summary</template-output>
</step>

</workflow>
