# Policy Section 4: Testing Strategy - Instructions

<critical>This document defines the principles, strategies, and protocols for testing. Adherence is mandatory to ensure software quality.</critical>
<critical>This is Section 4 of the AI Coding Agent Project Policy.</critical>

<workflow>

<step n="1" goal="Review Testing Overview and Principles">
    <action>Acknowledge the purpose of the testing strategy: to ensure testing is thoughtful, scoped, and documented.</action>
    <action>Internalize the key concept: Testing effort must align with complexity and risk, prioritizing quality.</action>
    <action>Adopt the principle of Risk-Based Testing: Prioritize tests based on feature complexity, risk, and criticality.</action>
    <action>Adopt the Test Pyramid Strategy: Focus on a large base of Unit Tests, a smaller layer of Integration Tests, and a few E2E Tests.</action>
    <action>Adopt the principle of Clarity and Maintainability: Ensure all tests are written to be clear, concise, and easy to maintain.</action>
    <action>Adopt the principle of Automation First: Automate all tests to ensure consistency, repeatability, and fast feedback.</action>
</step>

<step n="2" goal="Define Unit Test Scope">
    <action>Define Unit Test focus: individual functions, methods, or classes in isolation.</action>
    <action>Define Unit Test characteristics: fast, tests a single concern, has no external dependencies, and is predictable.</action>
    <action>Define Unit Test mocking strategy: Mock ALL external dependencies (e.g., databases, APIs). Do not test third-party library APIs.</action>
    <action>Define Unit Test scope: Verify internal logic, test edge cases, and validate error handling.</action>
</step>

<step n="3" goal="Define Integration Test Scope">
    <action>Define Integration Test focus: multiple components interacting as a subsystem.</action>
    <action>Identify what to test: API endpoints, service layers, database interactions, and message queue integrations.</action>
    <action>Define the mocking strategy for Integration Tests:</action>
    - Mock external third-party services (e.g., Firecrawl, Gemini).
    - Use REAL instances for internal infrastructure (e.g., a test database, a test message queue).
    <action>Acknowledge the rationale: Testing against real internal infrastructure is required to validate actual behavior and identify integration defects.</action>
</step>

<step n="4" goal="Define End-to-End (E2E) Test Scope">
    <action>Define E2E Test focus: complete application flows from a user's perspective.</action>
    <action>Acknowledge E2E Test characteristics: they are the slowest, most brittle, and have the highest maintenance cost.</action>
    <action>Define E2E Test scope: Reserve E2E tests for CRITICAL user paths and key business workflows ONLY.</action>
    <action>Identify examples of critical paths suitable for E2E testing:</action>
    - User registration-to-purchase flow.
    - Admin content publishing flow.
</step>

<step n="5" goal="Establish Test Plan Documentation Strategy">
    <action>Define PBI-Level testing requirements:</action>
    - Locate Conditions of Satisfaction (CoS) in the `prd.md` to define high-level success criteria.
    - Mandate that every PBI task list MUST include a dedicated E2E testing task (e.g., `<PBI-ID>-E2E-CoS-Test.md`) to verify CoS.
    <action>Apply the principle of Proportionality to task-level test plans: Test plans MUST be proportional to task complexity and risk.</action>
    <action>Define test plan scope for Simple Tasks (e.g., constants, types): a minimal plan focusing on compilation and basic integration.</action>
    <action>Define test plan scope for Basic Tasks (e.g., simple functions, CRUD): a moderate plan covering core functionality and error patterns.</action>
    <action>Define test plan scope for Complex Tasks (e.g., multi-service integration): a detailed plan covering comprehensive scenarios, edge cases, and a full mocking strategy.</action>
</step>

<step n="6" goal="Enforce Test Plan Requirements and Distribution">
    <action>Enforce the requirement: Every task involving code MUST include a `## Test Plan` section.</action>
    <action>Specify that test plan detail is determined by task complexity (Simple, Basic, or Complex).</action>
    <action>Enforce the completion prerequisite: A task CANNOT be marked `Done` unless the test plan is complete, all tests are implemented, and all tests are PASSING.</action>
    <action>Apply the Test Distribution Strategy to avoid duplication: Do NOT repeat detailed edge case testing across individual implementation tasks.</action>
    <action>Define focus for Task-Level tests: Verify that the specific functionality of the task works as intended.</action>
    <action>Define focus for E2E-Level tests: Concentrate complex scenarios, full workflow validation, and error scenarios in the dedicated E2E CoS test task.</action>
</step>

<step n="7" goal="Apply Test Implementation Guidelines">
    <action>Organize test files: Mirror the source code structure within the `test/unit` directory. Organize integration tests by feature.</action>
    <action>Follow naming conventions: Use `<source-file>.test.ts` for unit tests and `<feature-name>.test.ts` for integration tests.</action>
    <action>Structure tests logically: Use `describe` for components/methods and `it` for specific behaviors. Follow the Arrange-Act-Assert pattern.</action>
    <action>Adhere to best practices: Use clear, descriptive names for tests. Aim for one assertion per test where possible.</action>
</step>

<step n="8" goal="Initiate Test Failure Diagnosis Protocol">
    <action>Acknowledge the purpose: This protocol enables AI agents to handle test failures, with a focus on autonomous recovery.</action>
    <action>Execute the Test Failure Diagnosis Decision Tree upon test failure.</action>
    <action>Step 1: Identify failure type. Is it an Environment/Infrastructure Issue?</action>
    <check>If YES, proceed to the Auto-Retry Protocol.</check>
    <action>Step 2: If NO, is it a Code Defect (Logic Error)?</action>
    <check>If YES, proceed to the Fix Implementation Protocol.</check>
    <action>Step 3: If NO, is the Test Expectation Wrong?</action>
    <check>If YES, proceed to the Escalate to User Protocol.</check>
</step>

<step n="9" goal="Execute Auto-Retry Protocol for Environment Issues">
    <action>Condition: Execute this protocol for Environment/Infrastructure issues.</action>
    <action>Primary action: Attempt to auto-retry the tests with an exponential backoff.</action>
    <action>Execute retry process:</action>
    - **Attempt 1:** Immediate retry.
    - **Attempt 2:** If failure, wait 5 seconds and retry.
    - **Attempt 3:** If failure, wait 15 seconds and retry.
    <action>Log all retry attempts in the associated task.</action>
    <action>Attempt to auto-fix common issues (e.g., port conflicts, DB timeouts) before retrying.</action>
    <check>If all 3 retries fail:</check>
    <action>Mark the task as `Blocked`.</action>
    <action>Create a blocker document detailing the failure.</action>
    <action>Notify the User of the blocker.</action>
</step>

<step n="10" goal="Execute Fix Implementation Protocol for Code Defects">
    <action>Condition: Execute this protocol for Code Defects (Logic Errors).</action>
    <action>Primary action: Attempt to fix the implementation logic.</action>
    <action>Execute fix process:</action>
    1. Analyze the test failure to understand the root cause.
    2. Identify the specific function/method requiring correction.
    3. Implement the code fix.
    4. Re-run ALL tests to ensure the fix has not introduced regressions.
    5. Document the issue, root cause, and the implemented fix in the task documentation.
    <check>If the fix is unsuccessful after 3 attempts:</check>
    <action>Mark the task as `Blocked`.</action>
    <action>Escalate the issue to the User.</action>
</step>

<step n="11" goal="Execute Escalation Protocol for Invalid Test Expectations">
    <action>Condition: Execute this protocol when the test expectation appears to be wrong, not the implementation code.</action>
    <action>Primary action: Do NOT modify the test or the code. Escalate to the User.</action>
    <action>Execute escalation process:</action>
    1. Document the discrepancy, clearly stating the test failure and the conflict between implementation and test expectation.
    2. Provide context, showing the conflicting implementation logic and test expectation.
    3. Mark the task as `Blocked` with the reason "Test Expectation Clarification Needed".
    4. Notify the User, asking for confirmation of the correct behavior.
</step>

<step n="12" goal="Navigate Between Sections">
    <action>Use the following links to navigate the policy documents:</action>
    - [← Back to Index](../project-policy-index.md)
    - [← Previous: Section 3 - Task Management](./3-task-management.md)
    - [Next: Section 5 - Quick Reference →](./5-quick-reference.md)
</step>

</workflow>
