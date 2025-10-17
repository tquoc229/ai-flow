---
title: "[Feature] Short descriptive title"
labels: enhancement, feature
assignees: 
milestone: 
priority: medium
status: open
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# [Feature] Feature Name

## 📋 Summary

[Brief description of what this feature does and why it's needed]

---

## 🎯 Problem Statement

### Current Situation
[Describe what currently exists or what the problem is]

### User Impact
- **Who is affected:** [e.g., All users, Admin users, Mobile users]
- **How often:** [e.g., Daily, Weekly, Every login]
- **Severity:** [e.g., Blocker, Major inconvenience, Nice to have]

### Business Value
[Explain why this feature is important for the business/product]

---

## 💡 Proposed Solution

### Overview
[High-level description of the proposed solution]

### User Story

**As a** [type of user]  
**I want** [capability/feature]  
**So that** [benefit/value]

**Acceptance Scenario:**
```gherkin
Given [precondition]
When [user action]
Then [expected outcome]
```

---

## 🔧 Technical Design

### Architecture
```
[ASCII diagram or description]
```

### Key Components

| Component | Responsibility | Technology |
|-----------|---------------|------------|
| [Component 1] | [What it does] | [Tech stack] |

### API Changes

<details>
<summary>New Endpoints</summary>

**Endpoint:** `POST /api/v1/resource`

**Request:**
```json
{
  "field1": "value1"
}
```

**Response:**
```json
{
  "id": "123",
  "status": "success"
}
```

</details>

---

## ✅ Acceptance Criteria

### Functional Requirements
- [ ] **FR-1:** [Specific, testable requirement]
- [ ] **FR-2:** [Specific, testable requirement]

### Non-Functional Requirements
- [ ] **Performance:** [Measurable metric]
- [ ] **Security:** [Security requirement]
- [ ] **Accessibility:** [A11y requirement]

---

## 🧪 Testing Strategy

### Unit Tests
- [ ] Test [component A]
- [ ] Test [component B]

### Integration Tests
- [ ] Test [integration point]

---

## 📝 Implementation Plan

### Phase 1: Foundation (Estimated: X hours)

- [ ] **Task 1.1:** [Description]
  - Files: `path/to/file.ts`
  - Estimated: X hours

---

## 🏁 Definition of Done

- [ ] All acceptance criteria met
- [ ] Tests passing
- [ ] Code reviewed
- [ ] Documentation updated
- [ ] Deployed

---

## 📚 References & Resources

- [Link to documentation]
- [Link to design]
