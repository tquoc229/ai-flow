# Policy Section 4: Testing Strategy

<critical>This document defines testing principles, requirements, and proportionality for MyFlow projects.</critical>
<critical>This is Section 4 of the AI Coding Agent Project Policy.</critical>

---

## Overview

Testing is a **mandatory** part of task execution in MyFlow.

**Key Principles:**
- Tests must be proportional to task complexity
- Higher risk areas require more comprehensive testing
- All tests must be automated and repeatable
- Test results must be documented

---

## Test Proportionality Matrix

Test requirements scale with task complexity.

| Complexity | Unit Tests | Integration Tests | E2E Tests | Coverage Target |
|------------|------------|-------------------|-----------|-----------------|
| **Simple** | Optional | Not Required | Not Required | 60%+ |
| **Medium** | Required | Optional | Not Required | 70%+ |
| **Complex** | Required | Required | Optional | 80%+ |

**Configuration:** `config.yaml` → `testing.proportionality`

### How to Determine Complexity

**Simple:**
- Single function/method changes
- No external dependencies
- Clear, straightforward logic
- < 2 hours estimated work

**Medium:**
- Multiple functions/components
- Some external dependencies
- Moderate logic complexity
- 2-8 hours estimated work

**Complex:**
- System-wide changes
- Multiple external dependencies
- Complex business logic or algorithms
- > 8 hours estimated work

---

## Coverage Requirements

From `config.yaml` → `testing.coverage`:

```yaml
coverage:
  enabled: true
  minimum: 70       # Percentage
  strict: false     # Block on failure
```

**Targets:**
- **Minimum:** 70% (enforced)
- **Recommended:** 80%+
- **Critical paths:** 95%+

**Measurement:**
- Line coverage preferred
- Branch coverage for complex logic
- Auto-generated reports

---

## Test Types

### Unit Tests

**Purpose:** Test individual functions/methods in isolation

**Characteristics:**
- Fast execution (< 1ms per test)
- No external dependencies (mock all)
- Test one thing at a time
- Deterministic results

**When Required:**
- Medium and Complex tasks
- Functions with business logic
- Utility functions
- Data transformations

**When Optional:**
- Simple glue code
- Pass-through functions
- Trivial getters/setters

### Integration Tests

**Purpose:** Test component interactions and data flow

**Characteristics:**
- Moderate execution time (< 100ms per test)
- Real dependencies where practical
- Test interfaces between components
- May use test database/API

**When Required:**
- Complex tasks
- Multi-component features
- API endpoints
- Database operations

**When Optional:**
- Medium tasks with simple integration
- Internal component communication

### E2E (End-to-End) Tests

**Purpose:** Test complete user workflows

**Characteristics:**
- Slower execution (seconds per test)
- Real environment (or close simulation)
- Test critical user paths
- May be manual or automated

**When Required:**
- Complex tasks affecting user workflows
- Critical business processes
- Payment/authentication flows

**When Optional:**
- Most tasks (E2E suite maintained separately)

---

## Testing Principles

### 1. Test Pyramid

```
        ╱ E2E ╲       Few, slow, expensive
       ╱━━━━━━━╲
      ╱  Integ  ╲     Some, medium speed
     ╱━━━━━━━━━━━╲
    ╱    Unit     ╲   Many, fast, cheap
   ╱━━━━━━━━━━━━━━━╲
```

**Ratio:** 70% Unit : 20% Integration : 10% E2E (approximate)

### 2. Risk-Based Testing

Focus testing on:
- Critical paths (authentication, payment, data loss scenarios)
- High-change areas (frequently modified code)
- Complex logic (algorithms, calculations)
- External integrations (APIs, databases)

### 3. Automated Testing

ALL tests must be:
- Runnable via command line
- Executable in CI/CD
- Repeatable with same results
- Independent (no test dependencies)

### 4. Test Documentation

Every task must document:
- Test plan (in PRD for PBI, in task file for tasks)
- Test results (pass/fail counts, coverage %)
- Known issues or skipped tests

---

## Test Execution

### Auto-Detection

From `config.yaml` → `testing.test_command`:

```yaml
test_command: "auto"  # Auto-detect from package.json
```

**Common patterns:**
- `npm test`
- `npm run test`
- `yarn test`
- `pytest`
- `mvn test`

### Execution Modes

**Auto Mode:** (default)
- AI detects test command
- Runs automatically after implementation
- Reports results

**Manual Mode:**
- AI prompts user to run tests
- User reports results
- Used for complex setups

**Skip Mode:** (not recommended)
- Tests not executed during workflow
- Must document reason
- Creates technical debt

### Strict Mode

From `config.yaml` → `automation.halt_on_test_failure`:

```yaml
halt_on_test_failure: true
```

**When true:**
- Workflow HALTS if tests fail
- Task cannot move to InReview
- User must fix failures or override

**When false:**
- Test failures documented
- Workflow continues with warning
- Failures tracked as technical debt

---

## Test Implementation Guidelines

### Writing Unit Tests

**Good practices:**
- One assertion per test (preferably)
- Descriptive test names: `test_calculateTotal_withDiscount_returnsCorrectAmount`
- Arrange-Act-Assert pattern
- Mock external dependencies

**Example structure:**
```javascript
describe('Calculator', () => {
  describe('calculateTotal', () => {
    it('should return correct total with discount', () => {
      // Arrange
      const items = [10, 20, 30];
      const discount = 0.1;

      // Act
      const result = calculateTotal(items, discount);

      // Assert
      expect(result).toBe(54); // (60 * 0.9)
    });
  });
});
```

### Writing Integration Tests

**Good practices:**
- Test realistic scenarios
- Use test database/fixtures
- Clean up after tests
- Test error conditions

### Mocking Strategy

**When to mock:**
- External APIs
- Databases (for unit tests)
- File system operations
- Time/date functions
- Random generators

**When NOT to mock:**
- Internal modules (in integration tests)
- Simple utilities
- Configuration objects

---

## Test Failure Protocol

From policy Section 5 (Quick Reference).

**When tests fail:**

1. **Analyze failure** - Understand root cause
2. **Categorize:**
   - Implementation bug → Fix code
   - Test bug → Fix test
   - Expectation mismatch → Escalate to user

3. **In Strict Mode:**
   - HALT workflow
   - Fix issue
   - Re-run tests
   - Only proceed when passing

4. **Document** in implementation notes

---

## Workflows

For test execution details:

- **Execute Task**: `workflows/execute-task/`
  - Step 6: Write tests for each requirement
  - Step 8: Run complete test suite
  - Handles strict mode, coverage, reporting

---

## Related Sections

- [← Previous: Section 3 - Task Management](./3-task-management.md)
- [Next: Section 5 - Quick Reference →](./5-quick-reference.md)
- [← Back to Index](../project-policy-index.md)

---

**Version:** 3.0 (Refactored - Principles Only)
**Last Updated:** 2025-10-21

**Note:** For detailed test execution workflows, see `workflows/execute-task/`.
