[← Back to Index](../project-policy-index.md) | [← Previous: Section 3](./3-task-management.md)

---

# Section 4: Testing Strategy

**Contains:** Testing Overview, Principles, Test Scoping (Unit/Integration/E2E), Test Plan Documentation, Requirements, Distribution Strategy, Implementation Guidelines

---

## 5. Testing Strategy and Documentation

### 5.1 Overview

**Purpose:** Ensures testing is approached thoughtfully, appropriately scoped, and well-documented.

**Key Concepts:**
- Testing effort should match complexity and risk
- Test plans must be proportional to task complexity
- Avoid over-engineering test plans for simple tasks
- Focus on quality over quantity

---

### 5.2 Testing Principles

#### Risk-Based Testing

✅ **Prioritize testing based on:**
- Complexity of the feature
- Risk of failure and impact
- Criticality to system functionality
- Likelihood of change

#### Test Pyramid Strategy

```
        /\
       /E2E\      ← Few: Critical user paths only
      /------\
     /  INT   \   ← Some: Component interactions
    /----------\
   /    UNIT    \  ← Many: Individual functions
  /--------------\
```

**Distribution Guidelines:**
- **Unit Tests (Base):** Most numerous, fast, isolated
- **Integration Tests (Middle):** Moderate number, verify interactions
- **E2E Tests (Top):** Fewest, slowest, critical paths only

#### Clarity and Maintainability

✅ Tests should be:
- Clear and easy to understand
- Concise without complex logic
- Self-documenting with good names
- Easy to maintain and update

#### Automation First

✅ Automate all tests for:
- Consistent verification
- Repeatable execution
- Fast feedback loops
- Regression prevention

---

### 5.3 Test Scoping by Type

#### Unit Tests

**Focus:** Individual functions, methods, or classes **in isolation**

**Characteristics:**
- ✅ Test one thing at a time
- ✅ Fast execution (milliseconds)
- ✅ No external dependencies
- ✅ Predictable and deterministic

**Mocking Strategy:**
- ✅ Mock **ALL** external dependencies
- ✅ Mock database connections
- ✅ Mock API clients
- ❌ **DO NOT** test package API methods directly (covered by package's own tests)

**Scope:**
- ✅ Verify logic within the unit
- ✅ Test edge cases
- ✅ Test error handling
- ✅ Validate input/output transformations

**Example Scenarios:**
```javascript
// Unit test example
describe('calculateDiscount', () => {
  it('applies 10% discount for orders over $100', () => {
    const result = calculateDiscount(150);
    expect(result).toBe(15);
  });

  it('returns 0 for orders under $100', () => {
    const result = calculateDiscount(50);
    expect(result).toBe(0);
  });

  it('handles zero amount', () => {
    const result = calculateDiscount(0);
    expect(result).toBe(0);
  });
});
```

---

#### Integration Tests

**Focus:** Multiple components working together as a subsystem

**Characteristics:**
- ✅ Test component interactions
- ✅ Verify data flows correctly
- ✅ Test against real infrastructure when possible
- ✅ Moderate execution time (seconds)

**What to Test:**
- API endpoint + service layer + database + job queue
- Service orchestration across multiple services
- Data persistence and retrieval
- Message queue integration

**Mocking Strategy:**

✅ **Mock external third-party services:**
- External APIs (Firecrawl, Gemini, etc.)
- Third-party payment gateways
- External authentication services

✅ **Use REAL instances for internal infrastructure:**
- Database (test database instance)
- Message queues (pg-boss test instance)
- Cache (Redis test instance)

**Rationale:** Testing against real infrastructure validates actual behavior and catches integration issues.

**When to Start with Integration Tests:**
- ✅ Complex features with significant component interaction
- ✅ New workflows or orchestration logic
- ✅ Features spanning multiple services

**Example Scenarios:**
```javascript
// Integration test example
describe('User Registration Flow', () => {
  let testDb;
  let testQueue;

  beforeAll(async () => {
    testDb = await setupTestDatabase();
    testQueue = await setupTestQueue();
  });

  it('should register user and send welcome email', async () => {
    const userData = { email: 'test@example.com', password: 'secure123' };

    // Call API endpoint
    const response = await request(app)
      .post('/api/register')
      .send(userData);

    // Verify database
    const user = await testDb.users.findOne({ email: userData.email });
    expect(user).toBeDefined();

    // Verify job queued
    const jobs = await testQueue.fetch('send-email');
    expect(jobs).toHaveLength(1);
    expect(jobs[0].data.type).toBe('welcome');
  });
});
```

---

#### End-to-End (E2E) Tests

**Focus:** Complete application flow from user perspective

**Characteristics:**
- ✅ Test through UI (typically)
- ✅ Simulate real user interactions
- ✅ Slowest execution (minutes)
- ✅ Most brittle, highest maintenance

**Scope:**
- ✅ Reserved for **critical user paths** only
- ✅ Happy path scenarios
- ✅ Key business workflows
- ❌ **NOT** for every feature or edge case

**Example Scenarios:**
- User registration → login → purchase → checkout
- Admin creates content → publishes → user views
- Critical security flows

**Best Practices:**
- ✅ Keep E2E tests minimal
- ✅ Focus on business value
- ✅ Use page object pattern
- ✅ Make tests resilient to UI changes

---

### 5.4 Test Plan Documentation Strategy

#### PBI-Level Testing

**Conditions of Satisfaction (CoS):**

Located in `docs/delivery/<PBI-ID>/prd.md`, the CoS define:
- ✅ High-level success criteria for the PBI
- ✅ What "done" means for the PBI
- ✅ Acceptance criteria from User perspective

**No Need to Duplicate:** Detailed test plans are **NOT** duplicated at PBI level if covered by tasks.

**E2E CoS Test Task (Required):**

Every PBI task list **MUST** include a dedicated E2E testing task:

**Naming Pattern:** `<PBI-ID>-E2E-CoS-Test.md`

**Purpose:**
- ✅ Holistic end-to-end testing
- ✅ Verify PBI's overall CoS are met
- ✅ Test workflows spanning multiple implementation tasks
- ✅ Validate complete feature from user perspective

**Example:**
```markdown
# 6-E2E-CoS-Test: Verify Circuit Breaker Resilience

## Test Plan

### Objective
Verify the complete circuit breaker implementation meets all CoS for PBI-6.

### Test Scenarios

1. **Circuit Opens on Repeated Failures**
   - Simulate 5 consecutive failures
   - Verify circuit opens
   - Confirm requests are immediately rejected

2. **Circuit Recovers After Cooldown**
   - Wait for cooldown period
   - Verify circuit moves to half-open
   - Confirm successful request closes circuit

3. **Metrics and Monitoring**
   - Verify all state transitions logged
   - Confirm metrics captured correctly
   - Validate alerts triggered appropriately
```

---

#### Task-Level Test Plan Proportionality

**⚠️ CRITICAL PRINCIPLE:** Test plans **MUST** be proportional to task complexity and risk.

**Complexity Assessment:**

| Complexity | Characteristics | Test Plan Detail |
|------------|-----------------|------------------|
| **Simple** | Constants, interfaces, configuration | Minimal - compilation and basic integration |
| **Basic** | Simple functions, basic integrations | Moderate - core functionality and error patterns |
| **Complex** | Multi-service integration, complex logic | Detailed - scenarios, edge cases, mocking strategy |

---

##### Simple Tasks - Minimal Test Plans

**When to Use:**
- Defining constants or configuration
- Creating type definitions or interfaces
- Simple utility functions
- Basic model definitions

**Test Plan Focus:**
- ✅ Compilation success
- ✅ Basic integration works
- ✅ No syntax errors

**Example:**

```markdown
## Test Plan

### Objective
Verify constant definitions compile correctly and are accessible.

### Test Approach
- TypeScript compilation passes without errors
- Constants can be imported by other modules
- Values are correctly typed

### Success Criteria
- ✅ `npm run build` succeeds
- ✅ No TypeScript errors
- ✅ Constants accessible in dependent modules
```

**What to Avoid:**
- ❌ Elaborate test scenarios for simple constants
- ❌ Multiple test files for basic definitions
- ❌ Over-engineering for straightforward tasks

---

##### Basic Tasks - Moderate Test Plans

**When to Use:**
- Basic function implementation
- Simple service methods
- Straightforward integrations
- Standard CRUD operations

**Test Plan Focus:**
- ✅ Core functionality works
- ✅ Basic error handling
- ✅ Follows existing patterns

**Example:**

```markdown
## Test Plan

### Objective
Verify user validation function works correctly.

### Test Scope
- `validateUserInput()` function
- Error handling for invalid inputs

### Test Scenarios

1. **Valid Input**
   - Provide valid user data
   - Expect: Validation passes

2. **Invalid Email**
   - Provide malformed email
   - Expect: Validation error returned

3. **Missing Required Fields**
   - Omit required fields
   - Expect: Specific error messages

### Success Criteria
- All test scenarios pass
- Error messages are clear and helpful
- Function follows existing validation patterns
```

---

##### Complex Tasks - Detailed Test Plans

**When to Use:**
- Multi-service integration
- Complex business logic
- New architectural patterns
- Critical system functionality

**Test Plan Focus:**
- ✅ Comprehensive scenario coverage
- ✅ Detailed mocking strategy
- ✅ Edge cases and error scenarios
- ✅ Performance considerations

**Example:**

```markdown
## Test Plan

### Objective
Verify circuit breaker implementation handles all failure scenarios correctly.

### Test Scope
- Circuit breaker state machine
- Integration with retry mechanism
- Database connection handling
- Job queue integration

### Environment & Setup
- Test database with sample data
- Mock external API (Firecrawl)
- Real pg-boss instance for job queue
- Metrics collection enabled

### Mocking Strategy

**External Services:**
- ✅ Firecrawl API: Full mock with configurable responses
- ✅ Gemini API: Mock to simulate rate limiting

**Internal Infrastructure:**
- ✅ Database: Real PostgreSQL test instance
- ✅ Job Queue: Real pg-boss test instance
- ✅ Cache: Real Redis test instance

### Key Test Scenarios

#### 1. Happy Path - Circuit Remains Closed
**Setup:**
- Circuit starts in closed state
- External service responding normally

**Steps:**
1. Execute 10 consecutive successful requests
2. Verify all requests complete successfully
3. Check circuit state remains closed

**Expected:**
- All requests succeed
- Circuit state: Closed
- Metrics show 0 failures

---

#### 2. Circuit Opens on Repeated Failures
**Setup:**
- Circuit in closed state
- Configure failure threshold: 5 failures

**Steps:**
1. Simulate 5 consecutive failures from external service
2. Verify circuit opens after 5th failure
3. Attempt 6th request
4. Confirm request immediately rejected (circuit open)

**Expected:**
- Circuit state: Open after 5 failures
- 6th request fails immediately without calling service
- Metrics record failure count and state change

---

#### 3. Half-Open State and Recovery
**Setup:**
- Circuit in open state
- Wait for cooldown period (30 seconds)

**Steps:**
1. Wait for cooldown timeout
2. Verify circuit moves to half-open
3. Send successful test request
4. Confirm circuit closes

**Expected:**
- Circuit state transitions: Open → Half-Open → Closed
- Successful request in half-open closes circuit
- Metrics record state transitions

---

#### 4. Failure in Half-Open Reopens Circuit
**Setup:**
- Circuit in half-open state

**Steps:**
1. Send request that fails
2. Verify circuit reopens immediately
3. Confirm cooldown timer resets

**Expected:**
- Circuit state: Open
- New cooldown period started
- Metrics record reopening

---

#### 5. Concurrent Requests During State Transitions
**Setup:**
- Circuit approaching failure threshold

**Steps:**
1. Send 10 concurrent requests
2. Simulate failures on all requests
3. Verify circuit opens correctly
4. Confirm no race conditions

**Expected:**
- Circuit opens after threshold reached
- Subsequent requests handled correctly
- No inconsistent state

---

#### 6. Database Connection Resilience
**Setup:**
- Temporarily disconnect database

**Steps:**
1. Trigger operation requiring database
2. Verify circuit breaker activates
3. Reconnect database
4. Verify recovery after cooldown

**Expected:**
- Database errors trigger circuit breaker
- Circuit recovers when database available
- No data loss or corruption

### Edge Cases

1. **System restart during open state:**
   - Circuit state persists across restarts
   - Cooldown timer resumes correctly

2. **Very high request volume:**
   - Circuit breaker handles 1000+ req/sec
   - State transitions remain accurate

3. **Partial failures (some succeed, some fail):**
   - Failure rate calculated correctly
   - Circuit opens only when threshold exceeded

### Performance Requirements

- Circuit state check: < 1ms overhead
- State transition: < 5ms
- Metrics recording: Non-blocking

### Success Criteria

✅ All happy path scenarios pass
✅ All error scenarios handled correctly
✅ All edge cases covered
✅ Code coverage > 85% for circuit breaker module
✅ Integration tests pass with real infrastructure
✅ No memory leaks detected
✅ Performance requirements met
✅ Metrics accurately recorded
```

---

### 5.5 Test Plan Requirements

**Location:** Every task file in `## Test Plan` section

**Requirement:** Every task involving code implementation **MUST** include a test plan.

**Proportionality Matrix:**

| Task Type | Test Plan Detail | Key Elements |
|-----------|------------------|--------------|
| **Simple** | Minimal | Objective, basic success criteria |
| **Basic** | Moderate | Objective, test scope, 3-5 scenarios, success criteria |
| **Complex** | Comprehensive | All sections including mocking strategy, edge cases, performance |

**Test Plan Sections by Complexity:**

**Simple Tasks:**
```markdown
## Test Plan
- Objective: What are we verifying?
- Success Criteria: How do we know it works?
```

**Basic Tasks:**
```markdown
## Test Plan
- Objective
- Test Scope
- Test Scenarios (3-5)
- Success Criteria
```

**Complex Tasks:**
```markdown
## Test Plan
- Objective
- Test Scope
- Environment & Setup
- Mocking Strategy
- Key Test Scenarios (5-10)
- Edge Cases
- Performance Requirements (if applicable)
- Success Criteria
```

**⚠️ Completion Prerequisite:**

A task **CANNOT** be marked as `Done` unless:
- ✅ Test plan is complete and appropriate to complexity
- ✅ All tests in plan are implemented
- ✅ All tests are **PASSING**

**Review Guidelines:**
- Test plans reviewed for appropriateness to complexity
- Avoid over-engineering for simple tasks
- Ensure adequate coverage for complex tasks
- Update plan if requirements change significantly

---

### 5.6 Test Distribution Strategy

**Avoid Duplication:**

❌ **DO NOT** repeat detailed edge case testing across individual implementation tasks

✅ **DO** concentrate complex scenarios in dedicated E2E testing tasks

**Task-Level Testing Focus:**

Individual implementation tasks should verify:
- ✅ Specific functionality works as intended
- ✅ Integration with broader system
- ✅ Basic error handling
- ✅ Follows existing patterns

**E2E Testing Focus:**

Dedicated E2E CoS test tasks should verify:
- ✅ Complex integration testing
- ✅ Error scenarios across system
- ✅ Full workflow validation
- ✅ User acceptance criteria
- ✅ Performance and scalability

**Example Distribution:**

```
PBI-6: Circuit Breaker Implementation
├─ Task 6-1: Define state machine
│  └─ Test Plan: State transitions work correctly (unit tests)
├─ Task 6-2: Implement retry logic
│  └─ Test Plan: Retry mechanism works (unit + basic integration)
├─ Task 6-3: Add monitoring
│  └─ Test Plan: Metrics captured correctly (unit tests)
└─ Task 6-E2E-CoS-Test: Full circuit breaker testing
   └─ Test Plan: Complete scenarios, edge cases, integration tests
```

---

### 5.7 Test Implementation Guidelines

**Test File Organization:**

```
project/
├── src/
│   ├── services/
│   │   └── user.service.ts
│   └── utils/
│       └── validator.ts
└── test/
    ├── unit/              # Mirror source structure
    │   ├── services/
    │   │   └── user.service.test.ts
    │   └── utils/
    │       └── validator.test.ts
    └── integration/       # By feature or module
        ├── api/
        │   └── user-registration.test.ts
        └── workflows/
            └── circuit-breaker.test.ts
```

**Naming Conventions:**

✅ **Unit Tests:**
- Pattern: `<source-file>.test.ts`
- Example: `user.service.test.ts`

✅ **Integration Tests:**
- Pattern: `<feature-name>.test.ts`
- Example: `user-registration-flow.test.ts`

**Test File Structure:**

```typescript
describe('ComponentName', () => {
  // Setup
  beforeAll(() => {
    // One-time setup
  });

  beforeEach(() => {
    // Per-test setup
  });

  afterEach(() => {
    // Per-test cleanup
  });

  afterAll(() => {
    // One-time cleanup
  });

  // Test suites
  describe('methodName', () => {
    it('should do something when condition', () => {
      // Arrange
      const input = ...;

      // Act
      const result = methodName(input);

      // Assert
      expect(result).toBe(...);
    });
  });
});
```

**Best Practices:**

✅ **Clear test names:**
```typescript
// ❌ Bad
it('test1', () => { ... });

// ✅ Good
it('should return user when valid ID provided', () => { ... });
```

✅ **Arrange-Act-Assert pattern:**
```typescript
it('should calculate discount correctly', () => {
  // Arrange
  const orderAmount = 150;
  const expectedDiscount = 15;

  // Act
  const discount = calculateDiscount(orderAmount);

  // Assert
  expect(discount).toBe(expectedDiscount);
});
```

✅ **One assertion per test (when possible):**
```typescript
// Each test verifies one specific behavior
it('should return discount for orders over $100', () => {
  expect(calculateDiscount(150)).toBe(15);
});

it('should return zero for orders under $100', () => {
  expect(calculateDiscount(50)).toBe(0);
});
```

✅ **Descriptive test suites:**
```typescript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user with valid data', () => { ... });
    it('should throw error for duplicate email', () => { ... });
    it('should hash password before saving', () => { ... });
  });
});
```

---

### 5.8 Test Failure Protocol

**✨ NEW:** Comprehensive protocol for AI agents to handle test failures autonomously where possible.

---

#### Decision Tree: Test Failure Diagnosis

```
Tests failed during task execution
    ↓
Step 1: Identify failure type
    ↓
┌───────────────────────────────────────────────────────────┐
│ A. Environment/Infrastructure Issue                        │
│    - Database connection failed                           │
│    - Port already in use                                  │
│    - External service unreachable                         │
│    - Memory/resource exhaustion                           │
│    - Network timeout                                      │
└───────────────────────────────────────────────────────────┘
    ↓ YES → GO TO: Auto-Retry Protocol (below)
    ↓ NO → Continue to Step 2

┌───────────────────────────────────────────────────────────┐
│ B. Code Defect (Logic Error)                             │
│    - Assertion failed due to incorrect logic             │
│    - Unexpected return value                             │
│    - Exception thrown in implementation                  │
│    - Business logic violation                            │
└───────────────────────────────────────────────────────────┘
    ↓ YES → GO TO: Fix Implementation (below)
    ↓ NO → Continue to Step 3

┌───────────────────────────────────────────────────────────┐
│ C. Test Expectation Wrong                                │
│    - Test expects behavior that changed                  │
│    - Requirements evolved since test written             │
│    - Test assumptions no longer valid                    │
│    - Hardcoded test data out of sync                     │
└───────────────────────────────────────────────────────────┘
    ↓ YES → GO TO: Escalate to User (below)
```

---

#### Auto-Retry Protocol (Environment Issues)

**When:** Test failed due to environmental/infrastructure issue

**AI_Agent Actions:**

```markdown
1. **Identify Root Cause:**
   - Parse test output for error patterns
   - Check: Database down? Port conflict? Network issue?

2. **Execute Auto-Retry with Backoff:**

   Attempt 1: Immediate retry
   ↓
   SUCCESS? → Continue to submission
   FAIL? → Wait 5 seconds, retry
   ↓
   Attempt 2: After 5s delay
   ↓
   SUCCESS? → Continue to submission
   FAIL? → Wait 15 seconds, retry
   ↓
   Attempt 3: After 15s delay
   ↓
   SUCCESS? → Continue, log warning in task
   FAIL? → GO TO: Block Task (below)

3. **Log All Retry Attempts:**
   ```
   Test failures encountered:
   - Attempt 1 (10:00:00): DB connection timeout
   - Attempt 2 (10:00:05): DB connection timeout
   - Attempt 3 (10:00:20): Tests passed ✓
   ```

4. **If All Retries Fail:**
   - Update task status to Blocked
   - Create blocker document
   - Notify User
```

**Blocker Document Template:**

```markdown
## Task Blocked: Test Environment Issue

**Task:** <Task-ID>
**Blocked At:** <Timestamp>
**Issue Type:** Test Environment

**Problem:**
Tests consistently failing due to: <specific issue>

**Evidence:**
- Attempt 1: <error message>
- Attempt 2: <error message>
- Attempt 3: <error message>

**Root Cause Analysis:**
<AI analysis of what's wrong>

**Recommended Action:**
- [ ] Fix test database configuration
- [ ] Restart test services
- [ ] Check infrastructure logs
- [ ] <specific recommendation>

**Impact:**
Cannot proceed with task <ID> until environment stable.
```

---

#### Fix Implementation Protocol (Code Defects)

**When:** Test failed due to incorrect implementation logic

**AI_Agent Actions:**

```markdown
1. **Analyze Failure:**
   - Which test failed?
   - What was expected vs actual?
   - What business logic is violated?

2. **Identify Fix Location:**
   - Which function/method needs correction?
   - Is this a new bug or regression?
   - Does fix affect other tests?

3. **Implement Fix:**
   - Modify implementation code
   - Follow same coding standards
   - Add code comments if logic complex

4. **Re-run ALL Tests:**
   - Not just the failed test
   - Ensure fix didn't break other functionality
   - Verify integration tests still pass

5. **Document in Task:**
   ```markdown
   ## Implementation Issues Discovered

   **Issue:** Tests failed for calculateDiscount function
   **Root Cause:** Did not handle edge case for exactly $100 order
   **Fix Applied:** Updated condition from > to >= in line 42
   **Verification:** All 15 tests passing
   ```

6. **If Fix Successful:**
   - Continue to task submission
   - Log issue in task history

7. **If Fix Unsuccessful After 3 Attempts:**
   - Mark task Blocked
   - Escalate to User
   - Document what was attempted
```

---

#### Escalate to User Protocol (Test Expectation Issues)

**When:**
- Test expectation appears wrong (not implementation)
- Requirements may have changed
- AI uncertain if test or code is correct

**AI_Agent Actions:**

```markdown
1. **Document Discrepancy:**

   **Test Expectation Issue Detected**

   Test: test/user-service.spec.ts:42
   Failure: Expected user.role to be 'admin', got 'user'

   **Possible Reasons:**
   A. Implementation is wrong (role should be 'admin')
   B. Test is wrong (role should be 'user')
   C. Requirements changed since test written

2. **Provide Context:**

   **Implementation Logic:**
   ```typescript
   function createUser(data) {
     return { ...data, role: 'user' }; // Default role
   }
   ```

   **Test Expectation:**
   ```typescript
   expect(createUser({}).role).toBe('admin');
   ```

   **Question:** Should default role be 'user' or 'admin'?

3. **Mark Task as Blocked:**
   - Status: Blocked
   - Blocker Type: Test Expectation Clarification Needed
   - Waiting For: User decision on correct behavior

4. **Notify User:**
   Subject: Task <ID> - Test Expectation Clarification Needed

   "Test failure in <test name>. Implementation produces X,
    test expects Y. Please confirm correct behavior:

    Option A: Change implementation to match test
    Option B: Update test to match implementation
    Option C: Other (please specify)

    Details: <link to blocker document>"

5. **Wait for User Decision:**
   - Do not modify test or code without approval
   - User will unblock task with guidance
```

---

#### Common Environment Issues & Auto-Fixes

**AI_Agent can auto-fix these common issues:**

| Issue | Detection | Auto-Fix | Max Retries |
|-------|-----------|----------|-------------|
| **Database connection timeout** | Error: "ECONNREFUSED" | Restart test DB, retry | 3 |
| **Port already in use** | Error: "EADDRINUSE" | Kill process on port, retry | 2 |
| **Memory exhaustion** | Error: "JavaScript heap out of memory" | Increase heap size, retry | 1 |
| **Flaky network test** | Timeout on HTTP request | Retry with longer timeout | 3 |
| **File lock conflict** | Error: "EBUSY" | Wait 2s for release, retry | 3 |
| **Race condition** | Intermittent pass/fail | Add small delay, retry | 2 |

**Auto-Fix Script Example:**

```bash
# Port conflict auto-fix
if [[ $ERROR == *"EADDRINUSE"* ]]; then
  PORT=$(echo $ERROR | grep -oP ':\K\d+')
  lsof -ti:$PORT | xargs kill -9
  sleep 1
  npm test
fi
```

---

#### Test Failure Metrics (AI Tracking)

**AI_Agent should log these metrics:**

```markdown
## Test Failure Statistics

**Month:** October 2025

| Failure Type | Count | Auto-Recovered | Escalated | Avg Recovery Time |
|--------------|-------|----------------|-----------|-------------------|
| Environment  | 15    | 12 (80%)      | 3 (20%)  | 12 seconds       |
| Code Defect  | 8     | 8 (100%)      | 0 (0%)   | 45 minutes       |
| Test Expectation | 3 | 0 (0%)        | 3 (100%) | N/A              |

**Insights:**
- Most common: Database connection timeouts (8 occurrences)
- Auto-retry success rate: 80%
- Code defect fix success rate: 100%
- Average time from failure to recovery: 8 minutes
```

---

#### Summary: AI Agent Decision Flow

```
Test Failed
    ↓
Environment issue?
    YES → Auto-retry (max 3) → Success? YES → Continue
                              → Success? NO → Block & Notify
    ↓
    NO → Code defect?
        YES → Fix code → Re-run all tests → Pass? YES → Continue
                                           → Pass? NO → Try 2 more times
                                           → Still fail? Block & Notify
        ↓
        NO → Test expectation issue?
            YES → Document discrepancy → Block → Notify User
            ↓
            NO → Unknown issue → Block → Escalate with full context
```

**Key Principles:**
- ✅ Autonomous recovery for environmental issues
- ✅ Autonomous fix for clear code defects
- ✅ Always escalate when uncertain
- ✅ Never modify tests without User approval
- ✅ Log everything for debugging
- ✅ Track metrics to improve process

---

## Navigation

- [← Back to Index](../project-policy-index.md)
- [← Previous: Section 3 - Task Management](./3-task-management.md)
- [Next: Section 5 - Quick Reference →](./5-quick-reference.md)

---

**End of Section 4**
