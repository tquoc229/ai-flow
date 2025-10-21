# Create PBI - Validation Checklist

> **AI Agent**: Use this checklist to validate PBI creation before marking complete.
> Check each item and report results to the user.

---

## 📋 Pre-Creation Validation

### Context Loading ✓

- [ ] **AGENTS.md loaded** - Primary directive and critical rules understood
- [ ] **ai-flow-config.yaml loaded** - Central configuration and PBI states understood
- [ ] **Section 2 (PBI Management) loaded** - PBI lifecycle and rules understood
- [ ] **backlog.md loaded** - Current backlog structure and existing PBIs understood
- [ ] **Project documentation loaded** (for Legacy Discovery) - Architecture, APIs, components available

### Variable Collection ✓

- [ ] **user_story provided** - Follows format: "As a [actor], I want [action], so that [benefit]"
- [ ] **user_story validated** - Contains "As a", "I want", and "so that"
- [ ] **conditions_of_satisfaction provided** - At least 1 measurable criterion
- [ ] **priority set** - High, Medium, or Low
- [ ] **perform_legacy_discovery confirmed** - User chose yes/no
- [ ] **All required variables collected** - No missing inputs

### Backlog Analysis ✓

- [ ] **PBI table structure understood** - Columns identified
- [ ] **Maximum PBI ID identified** - Last ID extracted
- [ ] **Next PBI ID calculated** - max_id + 1
- [ ] **No ID conflicts** - Next ID is unique

---

## 🔄 During Creation Validation

### Initial PBI Entry (Proposed State) ✓

- [ ] **PBI ID assigned** - Unique, auto-incremented
- [ ] **Actor extracted** - From user story "As a [actor]"
- [ ] **Timestamp generated** - ISO 8601 format
- [ ] **Table row created** - Proper markdown format
- [ ] **Row inserted into backlog** - Table formatting preserved
- [ ] **Status set to Proposed** - Initial state correct
- [ ] **History entry created** - Logged in PBI History section
- [ ] **No other content modified** - Only table and history updated

### Legacy Discovery (if enabled) ✓

- [ ] **Search keywords extracted** - From user story and CoS
- [ ] **Targeted search performed** - Relevant directories searched
- [ ] **Reusable code identified** - Functions, components, patterns found
- [ ] **Findings documented** - File paths, functionality, reusability assessment
- [ ] **Implementation recommendations made** - Prioritize reuse vs create new
- [ ] **Findings stored** - Ready for PRD inclusion
- [ ] **Search was efficient** - Not loading entire codebase

### PRD Generation ✓

- [ ] **PRD directory created** - {output_folder}/{pbi_id}/
- [ ] **PRD file created** - {output_folder}/{pbi_id}/prd.md
- [ ] **YAML frontmatter complete** - All required fields present
- [ ] **All required sections present** - No missing sections
- [ ] **No placeholder text** - All sections filled (except "TBD" for estimated_hours)
- [ ] **Legacy Discovery Findings section included** - Findings documented
- [ ] **Implementation Plan prioritizes reuse** - Based on Legacy Discovery
- [ ] **Testing Strategy complete** - Unit, integration, E2E defined
- [ ] **Success Criteria mapped** - From Conditions of Satisfaction

### PRD Content Quality ✓

- [ ] **Context section meaningful** - Why, Current Situation, Desired State clear
- [ ] **Requirements specific** - Functional and non-functional clearly defined
- [ ] **Technical Approach detailed** - Architecture, components, tech stack described
- [ ] **Implementation Plan actionable** - Phases are clear and implementable
- [ ] **Testing Strategy comprehensive** - Maps to requirements
- [ ] **Success Criteria measurable** - Can determine when complete

### Bidirectional Links ✓

- [ ] **Backlog → PRD link created** - In backlog table row
- [ ] **PRD → Backlog link created** - In References & Notes section
- [ ] **Both links are valid** - Paths are correct
- [ ] **Links are clickable** - Proper markdown format

---

## ✅ Post-Creation Validation

### State Transition: Proposed → PlanInReview ✓

- [ ] **Timestamp generated** - ISO 8601 format
- [ ] **Status updated in backlog** - Proposed → PlanInReview
- [ ] **History entry created** - "PRD created" message logged
- [ ] **PRD frontmatter updated** - status: PlanInReview
- [ ] **PRD frontmatter timestamp** - updated field set
- [ ] **All transitions logged** - Complete history trail

### File Structure ✓

- [ ] **backlog.md updated** - New PBI row and history entry
- [ ] **PBI directory created** - {output_folder}/{pbi_id}/
- [ ] **prd.md created** - Complete PRD file
- [ ] **No extra files created** - Only expected files
- [ ] **File permissions correct** - Files are readable/writable

### Content Integrity ✓

- [ ] **Backlog structure preserved** - No accidental formatting changes
- [ ] **Existing PBIs untouched** - No modifications to other PBIs
- [ ] **PBI History section intact** - Only new entry added
- [ ] **PRD follows policy structure** - Matches Section 2 requirements
- [ ] **No data loss** - All user inputs captured

---

## 🔍 Quality Checks

### User Story Quality ✓

- [ ] **Actor is clear** - Identifiable persona/role
- [ ] **Action is specific** - Concrete, implementable request
- [ ] **Benefit is meaningful** - Clear business/user value
- [ ] **Story is independent** - Can be implemented standalone
- [ ] **Story is testable** - Can verify completion

### Conditions of Satisfaction Quality ✓

- [ ] **All criteria measurable** - Can objectively verify
- [ ] **No vague statements** - Specific, concrete criteria
- [ ] **Criteria are achievable** - Realistic expectations
- [ ] **Criteria are relevant** - Align with user story
- [ ] **At least 1 criterion** - Minimum requirement met

### Legacy Discovery Quality (if performed) ✓

- [ ] **Search was targeted** - Not exhaustive, focused on keywords
- [ ] **Findings are relevant** - Actually applicable to this PBI
- [ ] **Reusability assessed** - High/Medium/Low for each finding
- [ ] **Recommendations actionable** - Clear how to reuse components
- [ ] **No false positives** - Only truly reusable components listed
- [ ] **Implementation strategy clear** - When to reuse vs create new

### PRD Quality ✓

- [ ] **Requirements are complete** - Cover all CoS
- [ ] **Technical approach is sound** - Architecturally appropriate
- [ ] **Implementation plan is logical** - Phases in correct order
- [ ] **Testing strategy is adequate** - Covers all requirements
- [ ] **Success criteria are clear** - Unambiguous definition of "done"
- [ ] **Document is readable** - Well-formatted, clear language

---

## 📐 Policy Compliance

### PBI Management Policy ✓

- [ ] **Unique PBI ID** - No reuse, auto-incremented
- [ ] **Bidirectional links maintained** - Both directions present
- [ ] **Legacy Discovery performed** - MANDATORY (unless user opted out)
- [ ] **History logging complete** - All transitions logged with timestamp
- [ ] **backlog.md is source of truth** - Status authoritative
- [ ] **Valid state transitions** - Follows allowed transitions from config
- [ ] **PRD structure compliance** - All required sections present

### Configuration Compliance ✓

- [ ] **States match config** - Using states from ai-flow-config.yaml
- [ ] **Transitions valid** - Following pbi.transitions from config
- [ ] **Output folder correct** - Using {output_folder} from config
- [ ] **Document language correct** - Using {document_output_language}
- [ ] **Communication language used** - All messages in {communication_language}

---

## 📊 Success Criteria (Overall)

### Must-Have ✓

- [ ] **PBI created with unique ID** - In backlog.md
- [ ] **User story follows format** - As a, I want, so that
- [ ] **At least 1 CoS defined** - Measurable criterion
- [ ] **Status is PlanInReview** - After PRD creation
- [ ] **PRD file exists** - Complete and valid
- [ ] **Legacy Discovery documented** - Findings section present
- [ ] **Bidirectional links working** - Both directions valid
- [ ] **History complete** - All transitions logged

### Quality ✓

- [ ] **All sections complete** - No placeholder text
- [ ] **Implementation Plan actionable** - Can be decomposed into tasks
- [ ] **Testing Strategy comprehensive** - Adequate coverage defined
- [ ] **No policy violations** - Compliant with all rules
- [ ] **Document is professional** - Well-written, clear

### User Experience ✓

- [ ] **Progress updates shown** - User informed at each step
- [ ] **Errors handled gracefully** - Clear messages in {communication_language}
- [ ] **Summary provided** - Final report with links and next steps
- [ ] **Next steps clear** - User knows what to do next
- [ ] **Communication in correct language** - All messages in {communication_language}

---

## 📝 Validation Report Template

Use this template to report validation results to the user:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 PBI CREATION VALIDATION REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**PBI ID:** {pbi_id}
**Validation Date:** {timestamp}

**Checklist Results:**
- Pre-Creation: {passed}/{total} ✓
- During Creation: {passed}/{total} ✓
- Post-Creation: {passed}/{total} ✓
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
- Unique PBI ID
- User story format valid
- At least 1 CoS
- PRD file created
- Bidirectional links working
- History complete
- Status is PlanInReview

If any critical item fails, the PBI creation is considered incomplete.
