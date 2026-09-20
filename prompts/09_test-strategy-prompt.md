# Test Strategy and Test Plan Generator Prompt

> **Purpose**: Copy and paste this prompt into Kiro (or any AI coding assistant) to generate a test strategy document for any software project. The AI will analyze the existing test suite, test frameworks, coverage, and testing patterns to produce a document that explains what's tested, how, and what the standards are for adding new tests.
>
> **Standards**: Follows [ISTQB testing standards](https://www.istqb.org/) and your organization's internal documentation standards.

---

## The Prompt

```
I need you to generate a Test Strategy document for this project. This document should explain the testing approach, what's covered, how to run tests, how to add new tests, and what the quality standards are. Follow the process below systematically.

## Phase 1: Test Suite Analysis (Do This First — Report Before Writing)

Analyze the project and report findings. I will confirm before you begin writing.

### 1.1 Test Framework Inventory

Identify:
- **Test framework**: JUnit 5, Jest, pytest, Go test, etc. — with version
- **Mocking framework**: Mockito, Jest mocks, unittest.mock, etc. — with version
- **Assertion library**: AssertJ, Hamcrest, Chai, etc.
- **Specialized frameworks**: ApprovalTests, ArchUnit, Testcontainers, Selenium, Cypress, etc.
- **Test runner**: Maven Surefire, Gradle Test, npm test, pytest, etc.

### 1.2 Test Categorization

Catalog all test classes/files and categorize:
- **Unit tests**: Tests with mocked dependencies, fast execution
- **Integration tests**: Tests with real dependencies (database, services, containers)
- **Approval tests**: Tests that compare output against approved baselines
- **End-to-end tests**: Tests that exercise the full application stack
- **Performance tests**: Load tests, benchmark tests
- **Security tests**: Penetration tests, vulnerability scans

For each category, count:
- Number of test classes
- Number of test methods
- Approximate execution time

### 1.3 Test Tagging and Execution

Determine:
- **Test tags/categories**: How tests are categorized (@Tag, @Category, describe blocks, markers)
- **Execution commands**: How to run each category independently
- **CI/CD integration**: Which tests run in which pipeline stage
- **Parallel execution**: Whether tests run in parallel and how

### 1.4 Test Patterns and Conventions

Analyze existing tests for:
- **Naming conventions**: Test class names, test method names
- **Setup/teardown patterns**: @BeforeEach, @BeforeAll, fixtures, factories
- **Test data management**: Builders, factories, fixtures, seed data, approval files
- **Mocking patterns**: How dependencies are mocked (constructor injection, static mocking, reflection)
- **Assertion patterns**: Fluent assertions, custom matchers, approval verification

### 1.5 Coverage and Quality

Check for:
- **Coverage tool**: JaCoCo, Istanbul/nyc, coverage.py, etc.
- **Coverage targets**: Minimum coverage thresholds (from CI config or quality gates)
- **Current coverage**: If reports are available
- **Quality gates**: SonarQube, CodeClimate, or other quality gate configurations
- **Uncovered areas**: Classes or packages with no tests

### 1.6 Test Infrastructure

Identify:
- **Test resources**: Config files, approval files, test data files
- **Test utilities**: Helper classes, custom assertions, builders, reflection utilities
- **Test configuration**: Test-specific config (application-test.yml, test.properties)
- **External test dependencies**: Docker, databases, mock servers, test services

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Generate the Test Strategy Document

After confirmation, generate the document with the following structure. Skip sections that don't apply.

### Document Structure

```markdown
# Test Strategy — [Project Name]

## Overview

This document describes the testing strategy for [Project Name], including what's tested, how tests are organized, how to run them, and the standards for adding new tests.

---

## Test Pyramid

[Describe the project's test pyramid — what proportion of tests are unit vs. integration vs. e2e. Include a simple visual if helpful.]

```
        /  E2E  \          <- Few, slow, high confidence
       /----------\
      / Integration \      <- Some, moderate speed
     /----------------\
    /    Unit Tests     \  <- Many, fast, focused
   /____________________\
```

| Layer | Count | Execution Time | What It Tests |
|-------|-------|---------------|---------------|
| Unit | [count] | [time] | [scope] |
| Integration | [count] | [time] | [scope] |
| Approval | [count] | [time] | [scope] |
| E2E | [count] | [time] | [scope] |

---

## Test Frameworks

| Framework | Version | Purpose |
|-----------|---------|---------|
| [Framework] | [version] | [what it's used for] |

---

## Test Organization

### Directory Structure

```
src/test/
├── java/                    # Test source code
│   ├── [package]/           # Tests mirror source package structure
│   │   ├── *Test.java       # Unit and integration tests
│   │   └── *ApprovalTest.java  # Approval tests
│   └── utility/             # Test utilities
│       ├── approval/        # Approval test helpers
│       ├── builders/        # Test data builders
│       └── reflection/      # Reflection utilities for test setup
└── resources/
    └── approvals/           # Approval test baselines
```

### Naming Conventions

| Convention | Pattern | Example |
|-----------|---------|---------|
| Test class | `[ClassName]Test` | `PaymentProcessorTest` |
| Approval test class | `[ClassName]ApprovalTest` | `ReportRendererApprovalTest` |
| Test method | `[MethodName]_[Scenario]_[ExpectedResult]` or descriptive name | `testValidateAction_NullInput_ReturnsFalse` |

### Test Categories / Tags

| Tag | Description | Execution Command | CI Stage |
|-----|-------------|-------------------|----------|
| [tag] | [description] | [command] | [stage] |

---

## How to Run Tests

### All Tests

```bash
[command to run all tests]
```

### By Category

```bash
# Unit tests only (fast feedback)
[unit test command]

# Integration tests (requires [dependencies])
[integration test command]

# Approval tests
[approval test command]

# Specific test class
[single class command]

# Specific test method
[single method command]
```

### IDE Integration

[How to run tests from the IDE — run configurations, shortcuts, debugging]

---

## Test Patterns

### Unit Test Pattern

```[language]
// Standard unit test structure used in this project
[annotated example from the actual codebase showing the pattern]
```

**Key points**:
- [How dependencies are mocked]
- [How test data is created]
- [Assertion style]

### Approval Test Pattern

```[language]
// Approval test structure used in this project
[annotated example from the actual codebase]
```

**Key points**:
- [How approval output is generated]
- [Where approval files are stored]
- [How to update approvals when behavior changes intentionally]

### Mocking Pattern

```[language]
// How this project mocks dependencies
[annotated example]
```

**Key points**:
- [Mocking approach — constructor injection, static mocking, reflection]
- [When to use each approach]

---

## Test Data Management

### Builders

[How test data is created — builder pattern, factories, fixtures]

### Approval Files

[Where approval baselines are stored, how to update them, how to review changes]

### Test Configuration

[Test-specific configuration files and how they differ from production config]

---

## Adding New Tests

### When to Add Tests

- **New feature**: Add tests covering the happy path and key edge cases
- **Bug fix**: Add a test that reproduces the bug before fixing it
- **Refactoring**: Ensure existing tests pass; add tests if coverage gaps exist

### Step-by-Step: Adding a Unit Test

1. Create a test class in the matching package under `src/test/`
2. Name it `[ClassName]Test`
3. Add appropriate tags: `[tag examples]`
4. Follow the [unit test pattern](#unit-test-pattern) above
5. Run the test: `[command]`
6. Verify it passes and covers the intended behavior

### Step-by-Step: Adding an Approval Test

1. Create a test class named `[ClassName]ApprovalTest`
2. Follow the [approval test pattern](#approval-test-pattern) above
3. Run the test — it will fail on first run (no approved file yet)
4. Review the generated output in `[approval output location]`
5. If the output is correct, approve it: [approval process]
6. Run the test again — it should pass

### Test Review Checklist

When reviewing tests in code review:
- [ ] Test name clearly describes what's being tested
- [ ] Test has appropriate tags
- [ ] Test follows the project's established patterns
- [ ] Test covers the happy path and relevant edge cases
- [ ] Mocking is minimal — only mock what's necessary
- [ ] No test depends on execution order
- [ ] No test depends on external services (unless tagged as integration)
- [ ] Assertions are specific (not just "no exception thrown")

---

## Coverage

### Coverage Tool

[Tool name, how to generate reports, where reports are stored]

### Coverage Targets

| Metric | Target | Current |
|--------|--------|---------|
| Line coverage | [target]% | [current if known]% |
| Branch coverage | [target]% | [current if known]% |
| Method coverage | [target]% | [current if known]% |

### Generating Coverage Reports

```bash
[command to generate coverage report]
```

**Report location**: [where to find the HTML/XML report]

### What Doesn't Need Coverage

- [Generated code, configuration classes, simple getters/setters, etc.]

---

## CI/CD Integration

### Pipeline Test Stages

| Stage | Tests Run | Trigger | Duration |
|-------|-----------|---------|----------|
| [stage] | [which tests] | [when it runs] | [approximate time] |

### Handling Test Failures in CI

[What to do when tests fail in the pipeline — how to reproduce locally, how to check logs]

---

## Known Limitations and Notes

[Document any known test limitations, flaky tests, areas with low coverage, or testing debt]

---

*Last Updated: [DATE]*
```

## Phase 3: Validation

After generating the document, verify:

### Test Strategy Validation
- [ ] All test frameworks and versions match the actual project
- [ ] Test counts and categories match the actual test suite
- [ ] All test commands are real and would work if copy-pasted
- [ ] Test patterns include actual code examples from the project
- [ ] Naming conventions match the project's actual practice
- [ ] Coverage targets match the project's actual quality gates
- [ ] CI/CD integration matches the actual pipeline configuration
- [ ] "Adding New Tests" section is actionable and complete
- [ ] No secrets or sensitive data in test examples
- [ ] Links to existing documentation are correct

## Rules

1. **Read the actual test classes** — derive patterns from real tests, not assumptions
2. **Read the build manifest** — identify test framework versions and plugins
3. **Read the CI/CD config** — understand which tests run where
4. **Count the tests** — provide real numbers, not estimates
5. **Show real code examples** — use actual test code from the project (anonymized if needed)
6. **Don't invent coverage numbers** — only report coverage if you can verify it
7. **Link to existing docs** — reference architecture docs for understanding what's being tested
8. **Pause for confirmation** — after Phase 1 analysis, stop and wait before writing

## Reference Files

When generating this document, consult these project files if they exist:
- `pom.xml` / `package.json` — Test framework dependencies and versions
- `src/test/` — All test classes for pattern analysis
- `src/test/resources/` — Test resources, approval files, test configuration
- `src/test/java/utility/` — Test utility classes (builders, reflection, approval helpers)
- `.gitlab-ci.yml` / `.github/workflows/` — CI/CD test stages
- `Makefile` — Test execution shortcuts
- `.kiro/steering/junit5-tag-strategy.md` — JUnit 5 tag-based test categorization
- `.kiro/steering/java-standards.md` — Testing standards section
- `docs/architecture/02-endpoint-catalog.md` — Public API surface (what needs testing)
- `src/test/resources/approvals/notes/` — Testing notes with behavioral observations

## Organization Standards Reference

Standards and templates are maintained in your organization's standards repository (set its location for your team). Referenced files:
- `steering/junit5-tag-strategy.md` — JUnit 5 tag-based test categorization and CI/CD integration
- `steering/java-standards.md` — Testing standards, coverage targets
- `steering/security-standards.md` — Security testing requirements
```

---

## Usage Notes

### How to Use This Prompt

1. Open any project in your AI coding assistant
2. Copy the entire prompt above (everything between the outer triple backticks)
3. Paste it into the chat
4. The AI will analyze the test suite, report findings, and wait for confirmation
5. After confirmation, it generates the test strategy document

### Output Location

| Output | Suggested Location |
|---|---|
| Test Strategy | `docs/testing/test-strategy.md` or `docs/TEST_STRATEGY.md` |

### Relationship to Other Prompts

- **Developer Onboarding prompt** (`docs/prompts/developer-onboarding-guide-prompt.md`) — Onboarding guide references how to run tests; this explains the full strategy
- **Contributing Guide prompt** (`docs/prompts/contributing-guide-prompt.md`) — Contributing guide references test requirements; this explains them in detail
- **This prompt** — Generates the comprehensive test strategy and patterns document

---

**Last Updated**: 2026-04-23 (CST)
