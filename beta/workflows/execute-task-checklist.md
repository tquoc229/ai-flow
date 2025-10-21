# Execute Task - Validation Checklist

> **AI Agent**: Use this checklist to validate task execution before marking complete.
> Check each item and report results to the user.

---

## 📋 Pre-Execution Validation

### Context Loading ✓

- [ ] **AGENTS.md loaded** - Primary directive and critical rules understood
- [ ] **config.yaml loaded** - Central configuration understood
- [ ] **Project policy index loaded** - Policy overview understood
- [ ] **Section 3 (Task Management) loaded** - Task workflow rules understood
- [ ] **Task file loaded** - Requirements, implementation plan, verification steps extracted
- [ ] **Parent PBI PRD loaded** - PBI context and acceptance criteria understood
- [ ] **Task index loaded** - Current task statuses checked
- [ ] **Testing strategy loaded** (if applicable) - Test requirements understood

### Variable Extraction ✓

- [ ] **task_path provided** - Full path to task markdown file
- [ ] **pbi_id extracted** - Parent PBI ID identified
- [ ] **task_id extracted** - Task ID identified
- [ ] **All required variables collected** - No missing inputs

### Precondition Checks ✓

- [ ] **Task state is Agreed** - Verified from Status History table
- [ ] **No concurrent tasks** - Zero tasks in InProgress state (max_concurrent: 1)
- [ ] **Parent PBI in valid state** - ReadyForTasks or InProgress (or user confirmed override)
- [ ] **All required files exist** - Task file, PBI PRD, task index all readable
- [ ] **No policy violations detected** - All MyFlow policies compliant

---

## 🔄 During Execution Validation

### State Transition: Agreed → InProgress ✓

- [ ] **Timestamp generated** - ISO 8601 format
- [ ] **History entry created** - Proper format with all columns
- [ ] **Status History table updated** - New row inserted in task file
- [ ] **Table formatting preserved** - Markdown table still valid
- [ ] **Task index updated** - Status changed to InProgress in tasks.md
- [ ] **Backlog updated** (if enabled) - Task state reflected in backlog.md
- [ ] **No other content modified** - Only Status History table changed

### Requirements Implementation ✓

- [ ] **All requirements addressed** - Each item from task file implemented
- [ ] **Implementation matches plan** - Followed the Implementation Plan section
- [ ] **Files actually modified** - All files in "Files Modified" section changed
- [ ] **No unplanned changes** - Only modified files mentioned in task
- [ ] **Code quality maintained** - Follows project coding standards
- [ ] **No hardcoded values** - Configuration externalized appropriately
- [ ] **No secrets committed** - No API keys, credentials, or sensitive data

### Testing ✓

- [ ] **Tests written** - Unit/integration tests for new functionality
- [ ] **Test strategy followed** - Matches requirements from Section 4
- [ ] **Tests executed** - Run automatically or manually confirmed
- [ ] **All tests passing** - Zero failures (or failures documented/justified)
- [ ] **Coverage adequate** - Meets proportionality requirements for complexity
- [ ] **Test evidence captured** - Results documented in implementation notes
- [ ] **Edge cases covered** - Not just happy path

### Verification ✓

- [ ] **All verification steps executed** - From Verification section of task
- [ ] **Each verification passed** - Or failures documented
- [ ] **Evidence documented** - How each verification was performed
- [ ] **No skipped verifications** - All items checked (or user approved skip)

---

## ✅ Post-Execution Validation

### Documentation ✓

- [ ] **Implementation notes created** - Comprehensive summary of work done
- [ ] **Notes appended to task file** - Added at end, preserving structure
- [ ] **All changes documented** - Files created, modified, deleted listed
- [ ] **Test results included** - Test counts, coverage, pass/fail status
- [ ] **Verification results included** - Which items passed/failed
- [ ] **Technical notes added** - Important decisions, trade-offs, issues
- [ ] **Known issues documented** - Technical debt or workarounds noted
- [ ] **Next steps listed** - Clear guidance for reviewer

### State Transition: InProgress → InReview ✓

- [ ] **Timestamp generated** - ISO 8601 format
- [ ] **History entry created** - "Implementation complete" or similar message
- [ ] **Status History table updated** - Second row added to task file
- [ ] **Table formatting preserved** - Still valid markdown
- [ ] **Task index updated** - Status changed to InReview
- [ ] **Backlog updated** (if enabled) - Reflected in backlog.md
- [ ] **Final state is InReview** - Not InProgress or other state

### Git Commit (if created) ✓

- [ ] **Commit message follows format** - feat({pbi_id}): {task_title}
- [ ] **Message is descriptive** - Summarizes what was done
- [ ] **Correct files staged** - Only files from task
- [ ] **No unintended files** - No accidental inclusions
- [ ] **No AI attribution** - Per config: no AI signatures
- [ ] **Commit created successfully** - Hash captured
- [ ] **User approved commit** - Or auto_commit was enabled

---

## 🔍 Quality Checks

### Code Quality ✓

- [ ] **Follows project conventions** - Architecture patterns respected
- [ ] **No code duplication** - DRY principle applied
- [ ] **Appropriate abstractions** - Not over-engineered or under-engineered
- [ ] **Error handling present** - Edge cases and errors handled
- [ ] **Logging appropriate** - Debug/info logs where needed
- [ ] **Comments where needed** - Complex logic explained
- [ ] **No commented-out code** - Clean, production-ready code

### Testing Quality ✓

- [ ] **Tests are meaningful** - Actually test the functionality
- [ ] **Tests are maintainable** - Clear, readable test code
- [ ] **Proper test isolation** - Unit tests don't depend on external state
- [ ] **Mocking appropriate** - Used where needed, not overused
- [ ] **Assertions are clear** - Expected vs actual clearly defined
- [ ] **Test names descriptive** - Easy to understand what's being tested

### Documentation Quality ✓

- [ ] **Implementation notes are clear** - Anyone can understand what was done
- [ ] **Technical decisions explained** - Why certain approaches chosen
- [ ] **Instructions complete** - How to use/test the implementation
- [ ] **Known limitations noted** - What doesn't work or needs improvement
- [ ] **Links provided** - To relevant docs, issues, resources

---

## 📐 Policy Compliance

### MyFlow Policy ✓

- [ ] **Task-driven development** - Work tied to agreed task
- [ ] **Max concurrent: 1** - No other tasks in InProgress
- [ ] **State transitions valid** - Followed allowed transitions
- [ ] **History logged** - All transitions documented with timestamp
- [ ] **PRD alignment** - Implementation matches PBI requirements
- [ ] **No unauthorized changes** - Only changed what task specifies

### AGENTS.md Critical Rules ✓

- [ ] **Read AGENTS.md first** - Primary directive followed
- [ ] **No policy violations** - All critical rules respected
- [ ] **Task state validation** - Checked state before starting
- [ ] **Context fully loaded** - All required files read
- [ ] **User approval obtained** - At decision points
- [ ] **Common mistakes avoided** - Reviewed mistake list

### Testing Policy ✓

- [ ] **Test proportionality** - Tests match task complexity
- [ ] **Test pyramid followed** - More unit tests than integration
- [ ] **Risk-based testing** - Critical paths tested thoroughly
- [ ] **Coverage requirements met** - Meets minimums for complexity level

---

## 🎯 Final Success Criteria

### Mandatory (ALL must pass) ✓

- [ ] **✅ Task state: InReview** - Successfully transitioned
- [ ] **✅ All requirements implemented** - Zero pending items
- [ ] **✅ All tests passing** - Zero failures (or documented)
- [ ] **✅ All verifications passed** - Or failures justified
- [ ] **✅ Documentation complete** - Implementation notes thorough
- [ ] **✅ History logged** - Two entries in Status History
- [ ] **✅ Task index updated** - Reflects InReview state
- [ ] **✅ Structure preserved** - Task file format unchanged
- [ ] **✅ No concurrent tasks** - Still only 1 InProgress (now 0)
- [ ] **✅ Policy compliant** - All MyFlow rules followed

### Recommended (Should pass) ✓

- [ ] **⭐ Code quality high** - Maintainable, clean code
- [ ] **⭐ Test coverage good** - Above minimum thresholds
- [ ] **⭐ No technical debt** - Or documented if unavoidable
- [ ] **⭐ Git commit created** - Changes version controlled
- [ ] **⭐ User satisfied** - Happy with implementation

---

## 📊 Checklist Summary

**Total Items:** 100+

**Categories:**
- Pre-Execution: 20 items
- During Execution: 25 items
- Post-Execution: 20 items
- Quality Checks: 20 items
- Policy Compliance: 15 items
- Final Success Criteria: 15 items

**Passing Criteria:**
- **Mandatory items:** ALL must pass (marked with ✅)
- **Recommended items:** MOST should pass (marked with ⭐)

---

## 🤖 AI Agent Instructions for Using This Checklist

### When to Run This Checklist

Run this checklist at the end of Step 9 in execute-task-instructions.md, before displaying the final summary.

### How to Run

```
For each section:
  For each checklist item:
    1. Evaluate if the item passes based on work done
    2. Mark with [x] if passed, leave [ ] if failed
    3. For failed items, note the reason
  End for
End for

Display results to user:
- Total items checked
- Items passed
- Items failed (with reasons)
- Overall assessment
```

### Example Output

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 VALIDATION CHECKLIST RESULTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Pre-Execution Validation**
✅ 20/20 items passed

**During Execution Validation**
✅ 24/25 items passed
⚠️ 1 item warning:
  - Code coverage 68% (below 70% target, but acceptable)

**Post-Execution Validation**
✅ 20/20 items passed

**Quality Checks**
✅ 19/20 items passed
⚠️ 1 item warning:
  - Some complex logic could use more comments

**Policy Compliance**
✅ 15/15 items passed

**Final Success Criteria**
✅ 10/10 mandatory items passed
⭐ 4/5 recommended items passed

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**OVERALL ASSESSMENT: ✅ PASS**

- Mandatory criteria: ALL PASSED ✓
- Recommended criteria: MOST PASSED ⭐
- Warnings: 2 minor issues (documented above)

Task execution meets all requirements for InReview state.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Handling Failures

**If mandatory items fail:**
```
❌ VALIDATION FAILED

The following mandatory items did not pass:
- {item_1}: {reason}
- {item_2}: {reason}

Task cannot be marked as InReview until these are resolved.

Options:
1. Fix the issues now
2. Document as blockers and mark task as Blocked
3. Cancel task execution

→ Your choice:
```

**If only recommended items fail:**
```
⚠️ VALIDATION PASSED WITH WARNINGS

Some recommended items did not pass:
- {item_1}: {reason}

These are not blockers, but should be addressed if possible.

Proceed to mark task as InReview? (y/n)
```

---

## 📝 Notes for Continuous Improvement

### After Each Task Execution

Review this checklist and consider:
- Were all items relevant?
- Should any items be added?
- Should any items be removed?
- Should priorities change?

### Customization

This checklist can be customized per:
- Project type (web, mobile, backend, etc.)
- Team preferences
- Specific quality standards
- Industry requirements

---

**End of Checklist**

**Remember:** This checklist is a tool to ensure quality and compliance, not a bureaucratic burden. Use your judgment, adapt as needed, and always prioritize delivering value. ✅
