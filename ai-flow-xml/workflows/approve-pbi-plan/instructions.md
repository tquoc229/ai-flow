# Approve PBI Plan - Workflow Instructions

<critical>The workflow execution engine is governed by: {project-root}/ai-flow-config.yaml</critical>
<critical>You MUST have already loaded and processed: {project-root}/workflows/approve-pbi-plan/workflow.yaml</critical>
<critical>Communicate in {communication_language} throughout plan review</critical>
<critical>Address user as {user_name} in greetings and summaries</critical>

<workflow>

<step n="0" goal="Load context and validate workflow.yaml">
<action>Verify that workflow.yaml has been loaded and processed</action>
<action>Extract standard config variables: {user_name}, {communication_language}, {output_folder}, {date}</action>
<check>IF workflow.yaml not loaded THEN HALT with error message</check>
<action>Greet {user_name} in {communication_language}</action>
</step>

<step n="1" goal="Collect required variables from user">
<ask>Which PBI plan would you like to review and approve?

Please provide the PBI ID:

Example: 14

→ PBI ID:</ask>

<action>Store user response as {pbi_id}</action>
<action>Validate that PBI ID is numeric</action>

<template-output>user_variables</template-output>
</step>

<step n="2" goal="Load required context files and validate PBI state">
<action>Load the following files in order:</action>

<action>1. Primary directive: {project-root}/AGENTS.md</action>
<action>2. Configuration: {project-root}/ai-flow-config.yaml</action>
<action>3. PBI management rules: {project-root}/docs/rules/sections/2-pbi-management.md</action>
<action>4. Backlog: {output_folder}/backlog.md</action>
<action>5. PRD file: {output_folder}/{pbi_id}/prd.md</action>

<action>Parse backlog.md to verify:
- PBI {pbi_id} exists
- Extract current PBI status
- Extract PBI user story
</action>

<check>IF PBI does not exist THEN
  Display error in {communication_language}:
  "❌ ERROR: PBI {pbi_id} not found in backlog

  Please verify the PBI ID and try again."

  HALT workflow execution
END IF</check>

<check>IF PBI status NOT IN ["PlanInReview", "NeedsPlanRework"] THEN
  Display warning in {communication_language}:
  "⚠️ WARNING: PBI is in '{current_state}' state

  This workflow is designed for reviewing plans in 'PlanInReview' or 'NeedsPlanRework' state.

  Current state: {current_state}

  Do you want to continue anyway? (y/n)"

  IF user says no THEN HALT workflow execution
END IF</check>

<action>Parse PRD file completely and extract:
- YAML frontmatter (status, priority, created, updated, estimated_hours)
- All sections content
- Legacy Discovery Findings
- Implementation Plan phases
- Requirements count
- Testing Strategy
- Success Criteria
</action>

<template-output>loaded_context</template-output>
</step>

<step n="3" goal="Present PRD summary to user for review">
<action>Display comprehensive PRD summary in {communication_language}:</action>

<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 PBI PLAN REVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**PBI ID:** {pbi_id}
**Current Status:** {current_status}
**Priority:** {priority}
**Created:** {created_date}
**Last Updated:** {updated_date}

**User Story:**
{user_story}

**Conditions of Satisfaction:**
{conditions_of_satisfaction}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📝 PRD SUMMARY

### Context
**Why:** {why_from_context}
**Current Situation:** {current_situation_summary}
**Desired State:** {desired_state_summary}

### Requirements
**Functional Requirements:** {functional_count} items
1. {requirement_1}
2. {requirement_2}
3. {requirement_3}
...

**Non-Functional Requirements:**
- Performance: {performance_req}
- Security: {security_req}
- Scalability: {scalability_req}

### Technical Approach

**Architecture:** {architecture_summary}

**Legacy Discovery Findings:**
Found {findings_count} reusable components:
- {component_1}: {description_1}
- {component_2}: {description_2}
...

**Key Components:**
- {component_1}
- {component_2}
...

**Tech Stack:**
{tech_stack_list}

### Implementation Plan

**Phase 1:** {phase_1_name}
{phase_1_summary}

**Phase 2:** {phase_2_name}
{phase_2_summary}

**Phase 3:** {phase_3_name}
{phase_3_summary}

...

**Total Phases:** {phase_count}

### Testing Strategy
- **Unit Tests:** {unit_test_categories}
- **Integration Tests:** {integration_scenarios}
- **E2E Tests:** {e2e_tests}

### Success Criteria
{criteria_count} measurable criteria:
- [ ] {criterion_1}
- [ ] {criterion_2}
- [ ] {criterion_3}
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**PRD File:** {output_folder}/{pbi_id}/prd.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<template-output>prd_summary</template-output>
</step>

<step n="4" goal="Get user decision - Approve, Reject, or Revise">
<ask>Please review the plan above.

What would you like to do?

1. **Approve** - Plan looks good, ready to proceed with task creation
2. **Request Revisions** - Plan needs changes (you'll provide specific feedback)
3. **View Full PRD** - See the complete PRD document before deciding
4. **Cancel** - Exit without making changes

→ Your choice (1/2/3/4):</ask>

<action>Store user response as {decision}</action>

<check>IF {decision} == "3" (View Full PRD) THEN
  Display full PRD content in {communication_language}

  Then ask again:
  "Now that you've reviewed the full PRD, what would you like to do?
  1. Approve
  2. Request Revisions
  4. Cancel

  → Your choice (1/2/4):"

  Update {decision}
END IF</check>

<check>IF {decision} == "4" (Cancel) THEN
  Display in {communication_language}:
  "ℹ️ Plan review cancelled. No changes made.

  PBI {pbi_id} remains in {current_status} state."

  HALT workflow execution
END IF</check>

<template-output>user_decision</template-output>
</step>

<step n="5" goal="Process approval OR rejection">
<check>IF {decision} == "1" (Approve) THEN
  Go to Step 6 (Process Approval)
ELSE IF {decision} == "2" (Request Revisions) THEN
  Go to Step 7 (Process Rejection/Revision Request)
END IF</check>
</step>

<step n="6" goal="Process Approval - Transition to ReadyForTasks">
<action>Generate ISO 8601 timestamp using {date}</action>

<action>Update backlog.md:
- Change PBI {pbi_id} status from {current_status} to "ReadyForTasks"
- Add history entry: | {timestamp} | {pbi_id} | Plan Approved | User approved PRD, ready for task decomposition | {user_name} |
</action>

<action>Update PRD frontmatter in {output_folder}/{pbi_id}/prd.md:
- Set status: ReadyForTasks
- Set updated: {timestamp}
</action>

<action>Confirm approval in {communication_language}:</action>
<example>
✅ PBI Plan Approved

- Status: {previous_status} → ReadyForTasks
- History: Logged with timestamp
- PRD status: Updated
- Backlog: Updated

PBI is now ready for task decomposition.
</example>

<template-output>approval_processed</template-output>

<action>Go to Step 8 (Display Final Summary)</action>
</step>

<step n="7" goal="Process Rejection - Transition to NeedsPlanRework">
<ask>Please provide specific feedback on what needs to be revised:

What aspects of the plan need improvement?
- Requirements unclear or incomplete?
- Technical approach not appropriate?
- Implementation plan needs more detail?
- Testing strategy insufficient?
- Other concerns?

Please be as specific as possible:

→ Your feedback:</ask>

<action>Store user response as {revision_feedback}</action>

<action>Generate ISO 8601 timestamp using {date}</action>

<action>Update backlog.md:
- Change PBI {pbi_id} status from {current_status} to "NeedsPlanRework"
- Add history entry: | {timestamp} | {pbi_id} | Plan Rejected | User requested revisions: {brief_summary} | {user_name} |
</action>

<action>Update PRD file {output_folder}/{pbi_id}/prd.md:
- Set status: NeedsPlanRework in frontmatter
- Set updated: {timestamp}
- Append "Revision Feedback" section at end:

## Revision Feedback

**Date:** {timestamp}
**Reviewer:** {user_name}

**Feedback:**
{revision_feedback}

**Status:** NeedsPlanRework - awaiting AI revision

---
</action>

<action>Confirm rejection in {communication_language}:</action>
<example>
📝 Revision Requested

- Status: {previous_status} → NeedsPlanRework
- Feedback: Captured in PRD
- History: Logged with timestamp
- Backlog: Updated

AI will revise the plan based on your feedback.
</example>

<template-output>rejection_processed</template-output>

<action>Go to Step 8 (Display Final Summary)</action>
</step>

<step n="8" goal="Display final summary and next steps">
<action>Show comprehensive summary to {user_name} in {communication_language}:</action>

<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ PBI PLAN REVIEW COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**PBI ID:** {pbi_id}
**Previous Status:** {previous_status}
**New Status:** {new_status}
**Decision:** {Approved/Revision Requested}
**Timestamp:** {timestamp}

{IF approved}
**✅ PLAN APPROVED**

The PRD has been approved and the PBI is ready for task decomposition.

**Files Updated:**
- ✅ {output_folder}/backlog.md (status updated)
- ✅ {output_folder}/{pbi_id}/prd.md (status updated)

**State Transition:**
- {previous_status} → ReadyForTasks

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📋 Next Steps:**

1. **Decompose into Tasks**
   - Use `*decompose-tasks` workflow
   - Provide PBI ID: {pbi_id}
   - Tasks will be auto-approved and ready for execution

2. **Review Task Breakdown** (after decomposition)
   - Check generated tasks make sense
   - Verify task breakdown is logical

3. **Execute Tasks**
   - Use `*execute-task` workflow for each task
   - Complete tasks in order
   - Follow max_concurrent: 1 rule

{ELSE IF rejected}
**📝 REVISION REQUESTED**

The plan needs revisions based on your feedback.

**Feedback Provided:**
{revision_feedback}

**Files Updated:**
- ✅ {output_folder}/backlog.md (status updated)
- ✅ {output_folder}/{pbi_id}/prd.md (status updated, feedback added)

**State Transition:**
- {previous_status} → NeedsPlanRework

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📋 Next Steps:**

1. **AI Will Revise Plan**
   - AI will read your feedback
   - Revise PRD sections as needed
   - Address all concerns raised

2. **Review Revised Plan**
   - AI will notify when revisions complete
   - Review updated PRD
   - Use this workflow again to approve

3. **Iterate if Needed**
   - Request more revisions if necessary
   - Approve when satisfied

{END IF}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📂 Quick Links:**

- Backlog: [{output_folder}/backlog.md]({output_folder}/backlog.md)
- PRD: [{output_folder}/{pbi_id}/prd.md]({output_folder}/{pbi_id}/prd.md)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Workflow execution completed successfully! 🎉**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<template-output>final_summary</template-output>
</step>

</workflow>
