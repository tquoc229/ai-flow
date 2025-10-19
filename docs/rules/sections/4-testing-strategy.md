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

## Navigation

- [← Back to Index](../project-policy-index.md)
- [← Previous: Section 3 - Task Management](./3-task-management.md)
- [Next: Section 5 - Quick Reference →](./5-quick-reference.md)

---

**End of Section 4**
