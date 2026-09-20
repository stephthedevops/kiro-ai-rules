# Troubleshooting Guide Generator Prompt

> **Purpose**: Copy and paste this prompt into Kiro (or any AI coding assistant) to generate a troubleshooting guide for any software project. The AI will analyze the codebase, configuration, external dependencies, and common failure modes to produce a guide that helps developers and operators diagnose and resolve issues quickly.
>
> **Standards**: Follows symptom-based troubleshooting best practices and your organization's internal documentation standards.

---

## The Prompt

```
I need you to generate a comprehensive Troubleshooting Guide for this project. This guide should help developers and operators diagnose and resolve common issues during development, testing, and production operation. Follow the process below systematically.

## Phase 1: Failure Mode Analysis (Do This First — Report Before Writing)

Analyze the project and report findings. I will confirm before you begin writing.

### 1.1 Build and Compilation Failures

Identify potential build failures by analyzing:
- **Build manifest**: Dependency conflicts, version mismatches, missing repositories
- **Compiler settings**: Language version constraints, deprecated API usage
- **Build plugins**: Formatting checks, static analysis gates, code generation
- **CI/CD config**: Pipeline-specific build settings that differ from local

### 1.2 External Dependency Failures

For each external system the project integrates with, identify:
- **Connection failures**: What happens when the service is unreachable?
- **Authentication failures**: What happens when credentials are wrong or expired?
- **Timeout behavior**: What are the timeout settings and what happens on timeout?
- **Fallback behavior**: Does the code have fallbacks for unavailable services?
- **Error messages**: What error messages or exceptions indicate this failure?

### 1.3 Configuration Failures

Analyze:
- **Missing configuration**: What happens when config files are missing or incomplete?
- **Wrong environment**: What happens when the wrong environment profile is active?
- **Invalid values**: What happens with malformed config values?
- **Secrets/credentials**: What happens when Vault/secrets are unavailable?

### 1.4 Runtime Failures

Look for:
- **Null pointer risks**: Unguarded null access patterns
- **Resource exhaustion**: File handles, connections, memory, thread pools
- **Concurrency issues**: Race conditions, deadlocks, thread safety problems
- **External tool failures**: CLI tools not found, wrong version, permission denied

### 1.5 Test Failures

Identify:
- **Environment-dependent tests**: Tests that fail without Docker, network, or specific config
- **Flaky tests**: Tests with timing dependencies or external service calls
- **Approval test mismatches**: How to update approval files when behavior changes intentionally
- **Test data issues**: Missing fixtures, stale test data, database state

### 1.6 Deployment and Operations Failures

Check for:
- **Deployment failures**: Missing environment variables, wrong artifact version
- **Startup failures**: Application server configuration, classpath issues
- **Health check failures**: What health checks exist and what causes them to fail
- **Log analysis**: Where logs are written, how to find error details

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Generate the Troubleshooting Guide

After confirmation, generate the guide with the following structure. Skip sections that don't apply.

### Document Structure

```markdown
# Troubleshooting Guide — [Project Name]

## Overview

This guide covers common issues encountered during development, testing, and operation of [Project Name]. Issues are organized by category and presented in symptom-cause-solution format.

**Before troubleshooting**: Ensure your environment is set up correctly per the [Developer Guide](link) and that you're on the correct branch with a clean build.

---

## Quick Diagnostic Checklist

When something goes wrong, check these first:

- [ ] Are you on the correct branch? (`git branch --show-current`)
- [ ] Is your build clean? (`[build command]`)
- [ ] Are all tests passing? (`[test command]`)
- [ ] Is the correct environment/profile active?
- [ ] Are external services reachable? (VPN, network, service health)
- [ ] Are credentials valid and not expired?

---

## Build Issues

### Problem: [Symptom — what the developer sees]

**Error message**:
```
[Exact error message or pattern]
```

**Cause**: [Why this happens]

**Solution**:
1. [Step-by-step fix]
2. [Additional steps if needed]

**Prevention**: [How to avoid this in the future]

---

[Repeat for each build issue]

---

## Configuration Issues

### Problem: [Symptom]

**Error message**:
```
[Exact error message or pattern]
```

**Cause**: [Why this happens]

**Solution**:
1. [Step-by-step fix]

---

[Repeat for each configuration issue]

---

## External Service Issues

### [Service Name]

#### Problem: [Service] is unreachable

**Symptoms**:
- [What the developer/user sees]
- [Error messages in logs]

**Cause**: [Network, VPN, service down, DNS, firewall]

**Diagnosis**:
```bash
# Commands to diagnose the issue
[diagnostic command]
```

**Solution**:
1. [Step-by-step fix]

**Workaround**: [How to work without this service — local mode, mock, etc.]

---

[Repeat for each external service]

---

## Test Issues

### Problem: [Test symptom]

**Error message**:
```
[Exact error or test failure output]
```

**Cause**: [Why this test fails]

**Solution**:
1. [Step-by-step fix]

---

[Repeat for each test issue]

---

## Runtime Issues

### Problem: [Symptom]

**Error message**:
```
[Exact error message, stack trace pattern, or log entry]
```

**Cause**: [Why this happens]

**Diagnosis**:
```bash
# Commands or log locations to investigate
[diagnostic command]
```

**Solution**:
1. [Step-by-step fix]

---

[Repeat for each runtime issue]

---

## CI/CD Pipeline Issues

### Problem: [Pipeline symptom]

**Where to look**: [Pipeline URL pattern, job name, log location]

**Cause**: [Why this pipeline stage fails]

**Solution**:
1. [Step-by-step fix]

---

[Repeat for each pipeline issue]

---

## Log Locations and Analysis

### Where to Find Logs

| Log Type | Location | Format |
|----------|----------|--------|
| Application logs | [path or service] | [format] |
| Build logs | [path or CI/CD location] | [format] |
| Test logs | [path] | [format] |
| Server logs | [path] | [format] |

### Common Log Patterns

| Pattern | Meaning | Action |
|---------|---------|--------|
| `[ERROR pattern]` | [What it means] | [What to do] |
| `[WARN pattern]` | [What it means] | [What to do] |

---

## Escalation

If you can't resolve an issue using this guide:

1. Check the [architecture documentation](docs/architecture/) for system design context
2. Search the [issue tracker]([link]) for known issues
3. Contact the team: [team contact]

---

*Last Updated: [DATE]*
```

## Phase 3: Validation

After generating the guide, verify:

### Troubleshooting Guide Validation
- [ ] Every issue follows the symptom-cause-solution format
- [ ] Error messages are realistic (derived from actual code, not invented)
- [ ] Diagnostic commands are real and would work if copy-pasted
- [ ] Solutions are specific and actionable (not "check the configuration")
- [ ] External service issues include workarounds for local development
- [ ] Log locations are accurate
- [ ] Links to existing documentation (README, architecture docs, developer guide) are correct
- [ ] No secrets, passwords, or tokens appear anywhere
- [ ] Issues are organized by category (build, config, runtime, test, pipeline)
- [ ] The quick diagnostic checklist covers the most common root causes

## Rules

1. **Read the code** — derive failure modes from actual error handling, catch blocks, and validation logic
2. **Read the configuration** — understand what can go wrong with config files and environment setup
3. **Read the tests** — test notes and approval notes often document known issues and edge cases
4. **Read the CI/CD config** — understand pipeline-specific failure modes
5. **Use realistic error messages** — quote actual exception types and log patterns from the code
6. **Don't invent problems** — only document issues that could actually occur based on the codebase
7. **Include workarounds** — especially for external service dependencies during local development
8. **Link to existing docs** — reference architecture docs, config analysis, and integration maps
9. **Pause for confirmation** — after Phase 1 analysis, stop and wait before writing

## Reference Files

When generating this guide, consult these project files if they exist:
- `README.md` — Project overview, tech stack, known issues
- `docs/architecture/06-integration-map.md` — External systems and their connection details
- `docs/architecture/07-configuration-analysis.md` — Configuration properties and environment profiles
- `docs/architecture/05-security-architecture.md` — Authentication and authorization failure modes
- `docs/architecture/04-data-flow-maps.md` — Data flow paths where failures can occur
- `src/test/resources/approvals/notes/` — Testing notes documenting exceptions and edge cases
- `conf/` — Configuration files with environment-specific settings
- `.gitlab-ci.yml` / `.github/workflows/` — CI/CD pipeline configuration
- `pom.xml` / `package.json` — Dependencies that can cause build issues
- `.kiro/steering/security-standards.md` — Security-related failure modes

## Organization Standards Reference

Standards and templates are maintained in your organization's standards repository (set its location for your team). Referenced files:
- `steering/security-standards.md` — Security failure modes and compliance requirements
- `steering/java-standards.md` — Java-specific error patterns and best practices
- `steering/junit5-tag-strategy.md` — Test categorization and execution issues
```

---

## Usage Notes

### How to Use This Prompt

1. Open any project in your AI coding assistant
2. Copy the entire prompt above (everything between the outer triple backticks)
3. Paste it into the chat
4. The AI will analyze failure modes, report findings, and wait for confirmation
5. After confirmation, it generates the troubleshooting guide

### Output Location

| Output | Suggested Location |
|---|---|
| Troubleshooting Guide | `docs/troubleshooting.md` or `docs/TROUBLESHOOTING.md` |

### Relationship to Other Prompts

- **Developer Onboarding prompt** (`docs/prompts/developer-onboarding-guide-prompt.md`) — Onboarding guide has a brief troubleshooting section; this is the comprehensive version
- **Runbook prompt** (`docs/prompts/runbook-prompt.md`) — Runbooks cover operational procedures; this covers diagnosis and resolution
- **This prompt** — Generates the comprehensive troubleshooting reference

---

**Last Updated**: 2026-04-23 (CST)
