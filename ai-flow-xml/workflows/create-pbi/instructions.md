# Create PBI - Workflow Instructions

<critical>The workflow execution engine is governed by: {project-root}/ai-flow-config.yaml</critical>
<critical>You MUST have already loaded and processed: {project-root}/workflows/create-pbi/workflow.yaml</critical>
<critical>Follow PBI Management rules from: {project-root}/docs/rules/sections/2-pbi-management.md</critical>
<critical>Communicate in {communication_language} throughout PBI creation</critical>

<workflow>

<step n="0" goal="Load context and validate workflow.yaml">
<action>Verify that workflow.yaml has been loaded and processed</action>
<action>Extract standard config variables: {user_name}, {communication_language}, {output_folder}, {date}</action>
<check>IF workflow.yaml not loaded THEN HALT with error message</check>
<action>Greet {user_name} in {communication_language}</action>
</step>

<step n="1" goal="Load PBI Management policy and current backlog">
<action>Load PBI Management policy: {project-root}/docs/rules/sections/2-pbi-management.md</action>
<action>Load PRD template structure: {installed_path}/PRD_TEMPLATE.md</action>
<critical>PRD_TEMPLATE.md defines the EXACT format you MUST use when creating PRD files</critical>
<critical>Study the template structure, field descriptions, and examples carefully</critical>
<action>Load current backlog: {output_folder}/backlog.md</action>
<action>Parse backlog structure:
- Extract existing PBI IDs
- Identify highest PBI number
- Read PBI History section
- Understand current priorities
</action>

<action>Display context summary in {communication_language}:</action>
<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 PBI CREATION CONTEXT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Current Backlog Status:**
- Total PBIs: {count}
- Last PBI ID: {last_id}
- Next available ID: {next_id}

**PBI States Available:**
{list from config.pbi.states}

**Ready to create new PBI.**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<template-output>context_loaded</template-output>
</step>

<step n="2" goal="Collect PBI core information from user">
<ask>What is the user story for this PBI?

Format: As a [actor], I want [action], so that [benefit]

Example: As a developer, I want automated testing, so that I can catch bugs early

→ User story:</ask>

<action>Store as {user_story}</action>
<action>Validate format includes "As a", "I want", and "so that"</action>

<ask>What are the Conditions of Satisfaction (CoS)?

List measurable success criteria (one per line):
- Criterion 1
- Criterion 2
- ...

→ Conditions of Satisfaction:</ask>

<action>Store as {conditions_of_satisfaction}</action>
<check>IF no criteria provided THEN ask again - at least 1 is required</check>

<ask>What is the priority? [High/Medium/Low]

→ Priority [Medium]:</ask>

<action>Store as {priority} (default: "Medium")</action>

<template-output>user_inputs_collected</template-output>
</step>

<step n="3" goal="Create PBI entry in backlog - Transition to Proposed">
<action>Generate next PBI ID based on last ID in backlog</action>
<action>Generate ISO 8601 timestamp using {date}</action>

<action>Create new PBI table row:</action>
<example>
| {pbi_id} | {actor_from_user_story} | {user_story} | Proposed | {conditions_of_satisfaction} |
</example>

<action>Insert row into backlog.md PBI table (maintain alphabetical/numerical order if applicable)</action>

<action>Create PBI History entry:</action>
<example>
| {timestamp} | {pbi_id} | Created | Initial PBI creation by {user_name} | {user_name} |
</example>

<action>Append to PBI History section in backlog.md</action>

<action>Display confirmation in {communication_language}:</action>
<example>
✅ PBI Created Successfully

**PBI ID:** {pbi_id}
**Status:** Proposed
**User Story:** {user_story}
**Priority:** {priority}
**Conditions of Satisfaction:**
{conditions_of_satisfaction}

**Next Steps:**
User needs to approve this PBI idea before AI can generate a detailed plan.

To move forward:
1. Review the PBI in backlog.md
2. If approved, manually change status from "Proposed" → "PlanInReview"
3. AI will automatically detect change and generate detailed PRD
</example>

<template-output>pbi_created_proposed</template-output>
</step>

<step n="4" goal="Wait for user approval or generate plan immediately" optional="true">
<ask>Would you like me to generate the detailed plan (PRD) now?
- yes - Proceed to PlanInReview and generate PRD
- no - Stop here, you'll approve later

→ Your choice [no]:</ask>

<check>IF user says no THEN
  Display: "PBI creation complete. Status: Proposed. Workflow ends here."
  HALT workflow
END IF</check>

<check>IF user says yes THEN
  Display in {communication_language}:
  "Proceeding to plan generation. Changing status: Proposed → PlanInReview"

  Continue to Step 5
END IF</check>
</step>

<step n="5" goal="Transition to PlanInReview - Generate PRD">
<action>Update PBI status in backlog.md: Proposed → PlanInReview</action>
<action>Generate timestamp</action>
<action>Add history entry: "| {timestamp} | {pbi_id} | Generated Plan for Review | AI created detailed plan... | ai-agent |"</action>

<action>Display in {communication_language}:</action>
<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 LEGACY DISCOVERY PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Searching for existing code that can be reused...

This helps us:
- Avoid reinventing the wheel
- Maintain consistency
- Reduce development time
- Follow DRY principle

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<template-output>transition_to_plan_in_review</template-output>
</step>

<step n="6" goal="Perform Legacy Discovery - Find reusable code">
<action>Load codebase structure: {project-root}/docs/codebase-structure-architecture-code-standards.md</action>

<action>Targeted Legacy Discovery Process:
1. Identify key terms from user story and CoS
2. Search for relevant files/functions using those terms
3. Read ONLY the small set of potentially relevant code
4. Evaluate reusability (functions, components, patterns)
5. Document findings
</action>

<action>Search patterns to use:
- Grep for keywords from user story
- Look in relevant directories (based on architecture doc)
- Check for similar functionality in existing features
- Identify reusable utilities/helpers
</action>

<action>Document Legacy Discovery Findings:</action>
<example>
## Legacy Discovery Findings

**Search Keywords:** {keywords_from_user_story}

**Reusable Code Found:**
1. **File:** {file_path}
   **Functionality:** {what_it_does}
   **Reusability:** {how_can_reuse} (High/Medium/Low)
   **Notes:** {any_modifications_needed}

2. **File:** {file_path}
   ...

**Patterns Identified:**
- {pattern_1}: Used in {where_found}
- {pattern_2}: Can apply to {our_use_case}

**Recommendation:**
{prioritize_modifying_existing_vs_creating_new}
</example>

<template-output>legacy_discovery_findings</template-output>
</step>

<step n="7" goal="Create PBI directory and PRD file">
<action>Create directory: {output_folder}/{pbi_id}/</action>
<action>Create file: {output_folder}/{pbi_id}/prd.md</action>

<critical>You MUST follow the PRD template structure from {installed_path}/PRD_TEMPLATE.md</critical>
<critical>This template was loaded in Step 2 - review it to understand the required format</critical>

<action>Generate PRD content following PRD_TEMPLATE.md structure with all required sections:</action>

<example>
---
status: PlanInReview
priority: {priority}
created: {date}
updated: {date}
estimated_hours: TBD
---

# {title_from_user_story}

## Context

**Why do we need this?**
{extract_benefit_from_user_story_and_explain_problem_or_opportunity}

**Current Situation:**
{describe_current_state_what_exists_now}

**Desired State:**
{describe_desired_state_what_we_want}

## User Story

As a {actor}
I want {action}
So that {benefit}

**Example Scenario:**

```
Given {precondition}
When {user_action}
Then {expected_outcome}
```

## Requirements

### Functional Requirements

- [ ] REQ-1: {specific_requirement_1}
- [ ] REQ-2: {specific_requirement_2}
- [ ] REQ-3: {specific_requirement_3}
...

### Non-Functional Requirements

- [ ] Performance: {e.g._response_time_requirement}
- [ ] Security: {e.g._authentication_requirements}
- [ ] Scalability: {e.g._concurrent_users_support}

## Technical Approach

**Architecture:**

```
{describe_or_diagram_the_architecture}
```

---

### **Legacy Discovery Findings**

{paste_from_step_6}

**Findings Summary:**
- **File:** `{file_path_1}`
  - **Function/API:** `{function_name}()`
  - **Notes:** Will be reused/extended for {purpose}

- **Component:** `{component_path}`
  - **Notes:** Will be reused for {purpose}

---

**Key Components (To build/modify):**
[List new components to build OR existing components to modify, based on Discovery Findings above]

1. {component_1}: {purpose_and_what_to_do}
2. {component_2}: {purpose_and_what_to_do}
3. {component_3}: {purpose_and_what_to_do}

**Technology Stack:**

- Frontend: {frontend_technologies}
- Backend: {backend_technologies}
- Database: {database_technologies}
- External Services: {apis_libraries_services}

## Implementation Plan

### Phase 1: {phase_name}

1. Step 1: {description}
   - Files: {list_files_to_create_or_modify}
   - Estimated: {time_estimate}

2. Step 2: {description}
   - Files: {list_files}
   - Estimated: {time_estimate}

### Phase 2: {phase_name}

1. Step 1: {description}
   - Files: {list_files}
   - Estimated: {time_estimate}

...

## Testing Strategy

**Unit Tests:**
- {test_case_1}
- {test_case_2}
- {test_case_3}

**Integration Tests:**
- {test_scenario_1}
- {test_scenario_2}

**E2E Tests:**
- {user_flow_1}
- {user_flow_2}

## Success Criteria

- [ ] All functional requirements met
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Performance benchmarks met
{map_additional_criteria_from_conditions_of_satisfaction}

## References

- [Link to design doc or similar implementation]
- [Back to Backlog](../backlog.md)

## Notes

{any_additional_notes_constraints_or_considerations}
</example>

<action>Write PRD to {output_folder}/{pbi_id}/prd.md</action>

<template-output>prd_created</template-output>
</step>

<step n="8" goal="Create bidirectional links">
<action>In backlog.md, update PBI row to link to PRD:</action>
<example>
| {pbi_id} | ... | [PRD](./delivery/{pbi_id}/prd.md) | ... |
</example>

<action>Verify PRD has link back to backlog (already in template)</action>

<action>Confirm bidirectional links in {communication_language}:</action>
<example>
✅ Bidirectional Links Created

- Backlog → PRD: [PRD](./delivery/{pbi_id}/prd.md)
- PRD → Backlog: [Back to Backlog](../backlog.md)
</example>

<template-output>links_established</template-output>
</step>

<step n="9" goal="Display final summary and next steps">
<action>Show comprehensive completion summary to {user_name} in {communication_language}:</action>

<example>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ PBI CREATION COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**PBI ID:** {pbi_id}
**Status:** PlanInReview
**Priority:** {priority}

**User Story:**
{user_story}

**Conditions of Satisfaction:**
{conditions_of_satisfaction}

**Created Files:**
- {output_folder}/backlog.md (updated)
- {output_folder}/{pbi_id}/prd.md (created)

**Legacy Discovery:**
- Found {count} reusable code items
- Implementation plan prioritizes code reuse

**State Transitions:**
1. (none) → Proposed ({timestamp_1})
2. Proposed → PlanInReview ({timestamp_2})

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📋 Next Steps:**

1. **Review the PRD**
   - Open: {output_folder}/{pbi_id}/prd.md
   - Verify all sections are complete
   - Check Legacy Discovery findings

2. **Approve or Request Rework**
   - If approved: Change status to "ReadyForTasks" in backlog.md
   - If needs changes: Change status to "NeedsPlanRework" and provide feedback

3. **Task Decomposition** (after approval)
   - AI will decompose PRD into specific tasks
   - All tasks will be created with "Agreed" status

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**📂 Quick Links:**

- Backlog: [backlog.md]({output_folder}/backlog.md)
- PRD: [{pbi_id}/prd.md]({output_folder}/{pbi_id}/prd.md)
- Policy: [PBI Management](docs/rules/sections/2-pbi-management.md)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Workflow execution completed successfully! 🎉**

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
</example>

<template-output>final_summary</template-output>
</step>

</workflow>
