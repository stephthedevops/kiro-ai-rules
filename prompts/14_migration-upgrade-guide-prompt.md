# Migration and Upgrade Guide Generator Prompt

> **Purpose**: Copy and paste this prompt into Kiro (or any AI coding assistant) to generate a migration and upgrade guide for any software project that has undergone or is planning breaking changes across major versions. The AI will analyze the version history, breaking changes, API differences, and configuration changes to produce a guide that helps consumers upgrade safely.
>
> **Standards**: Follows [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html) migration best practices and your organization's internal documentation standards.

---

## The Prompt

```
I need you to generate a Migration and Upgrade Guide for this project. This guide should help consumers (downstream projects, teams, or users) upgrade between major versions safely, with clear instructions for handling breaking changes. Follow the process below systematically.

## Phase 1: Breaking Change Analysis (Do This First — Report Before Writing)

Analyze the project and report findings. I will confirm before you begin writing.

### 1.1 Version History

From git tags and CHANGELOG.md, identify:
- **All major versions**: With release dates
- **Breaking changes per major version**: What was removed, renamed, or changed incompatibly
- **Deprecation timeline**: Features deprecated in MINOR versions before removal in MAJOR
- **Migration notes**: Any existing migration documentation in README, CHANGELOG, or docs/

### 1.2 API Surface Changes

For each major version boundary, compare:
- **Removed classes/methods**: Public API that was removed
- **Renamed classes/methods**: Public API that was renamed
- **Changed signatures**: Methods with changed parameters, return types, or exceptions
- **Changed behavior**: Methods that behave differently (even with the same signature)
- **New required parameters**: Configuration or method parameters that are now mandatory

### 1.3 Configuration Changes

For each major version boundary, compare:
- **Removed config properties**: Properties that no longer exist
- **Renamed config properties**: Properties with new names
- **Changed defaults**: Properties with different default values
- **New required properties**: Properties that must now be set
- **Format changes**: Changes to config file format or structure

### 1.4 Dependency Changes

For each major version boundary, identify:
- **Removed dependencies**: Libraries that were dropped
- **Added dependencies**: New libraries that consumers may need to accommodate
- **Version bumps**: Dependencies with major version changes that may affect consumers
- **Transitive dependency changes**: Changes that affect the consumer's dependency tree

### 1.5 Consumer Impact Assessment

For each known consumer:
- **What breaks**: Specific code, config, or behavior that will break
- **Migration effort**: Estimated effort (trivial, moderate, significant)
- **Migration order**: If multiple consumers exist, what order should they migrate?
- **Rollback risk**: Can the consumer rollback if the migration fails?

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Generate the Migration Guide

After confirmation, generate the guide with the following structure.

### Document Structure

```markdown
# Migration and Upgrade Guide — [Project Name]

## Overview

This guide helps consumers of [Project Name] upgrade between major versions. Each section covers a specific version transition with step-by-step migration instructions.

**General upgrade strategy**: Always upgrade one major version at a time. Don't skip major versions.

---

## Table of Contents

- [Upgrade Path Summary](#upgrade-path-summary)
- [Migrating from X.x to Y.0](#migrating-from-xx-to-y0)
- [Migrating from W.x to X.0](#migrating-from-wx-to-x0)
- [FAQ](#faq)

---

## Upgrade Path Summary

| From | To | Breaking Changes | Migration Effort | Key Changes |
|------|-----|-----------------|-----------------|-------------|
| X.x | Y.0 | [count] | [Trivial/Moderate/Significant] | [One-line summary] |
| W.x | X.0 | [count] | [Trivial/Moderate/Significant] | [One-line summary] |

**Recommended upgrade path**: [Describe the recommended sequence, especially if intermediate versions are needed]

---

## Migrating from [Previous Major].x to [New Major].0

### Prerequisites

Before upgrading:
- [ ] You are on the latest MINOR/PATCH of [Previous Major].x
- [ ] All deprecation warnings from [Previous Major].x are resolved
- [ ] Your test suite passes on [Previous Major].x
- [ ] You have read the [CHANGELOG](CHANGELOG.md) entries for [New Major].0

### Breaking Changes

#### 1. [Breaking Change Title]

**What changed**: [Clear description of what was removed/changed]

**Why**: [Reason for the change]

**Before** ([Previous Major].x):
```[language]
// Old code or configuration
[old code]
```

**After** ([New Major].0):
```[language]
// New code or configuration
[new code]
```

**Migration steps**:
1. [Specific step to migrate this change]
2. [Verification step]

---

#### 2. [Breaking Change Title]

[Same format as above]

---

### Configuration Changes

| Property | [Previous Major].x | [New Major].0 | Action Required |
|----------|-------------------|---------------|-----------------|
| `old.property` | Used | Removed | Replace with `new.property` |
| `new.property` | N/A | Required | Add to config with value `[default]` |

### Dependency Changes

| Dependency | [Previous Major].x | [New Major].0 | Impact |
|-----------|-------------------|---------------|--------|
| [dep name] | [old version] | [new version or removed] | [What consumers need to do] |

### Step-by-Step Migration

1. **Update the dependency version** in your build manifest:
   ```xml
   <!-- Before -->
   <version>[old version]</version>
   <!-- After -->
   <version>[new version]</version>
   ```

2. **Update configuration**:
   - [Specific config changes]

3. **Update code**:
   - [Specific code changes]

4. **Build and test**:
   ```bash
   [build command]
   [test command]
   ```

5. **Verify**:
   - [ ] Application starts without errors
   - [ ] All tests pass
   - [ ] [Specific verification for this migration]

### Rollback Plan

If the migration fails:
1. Revert the dependency version to [Previous Major].x
2. Revert any configuration changes
3. Revert any code changes
4. Verify the application works on the previous version

---

[Repeat for each major version transition]

---

## FAQ

### Can I skip major versions?

[Answer — generally no, explain why]

### What if I'm still on a very old version?

[Answer — recommended upgrade path through intermediate versions]

### How do I know if I'm affected by a breaking change?

[Answer — how to check: compile errors, test failures, runtime behavior changes]

### What's the recommended testing strategy for upgrades?

[Answer — build, unit tests, integration tests, smoke tests in lower environments first]

---

*Last Updated: [DATE]*
```

## Phase 3: Validation

After generating the guide, verify:

### Migration Guide Validation
- [ ] Every breaking change has before/after code examples
- [ ] Every breaking change has specific migration steps
- [ ] Configuration changes are listed in a comparison table
- [ ] Dependency changes are listed with consumer impact
- [ ] Step-by-step migration is complete and actionable
- [ ] Build and test commands are real
- [ ] Rollback plan is included for each migration
- [ ] Version numbers match actual project versions
- [ ] No secrets, passwords, or tokens appear anywhere
- [ ] Links to CHANGELOG and other docs are correct

## Rules

1. **Read the CHANGELOG** — breaking changes are documented there
2. **Read git diffs between major versions** — compare actual code changes
3. **Read the API catalog** — understand what public API changed
4. **Read the configuration analysis** — understand what config properties changed
5. **Don't invent breaking changes** — only document changes that actually occurred
6. **Show before/after** — every breaking change needs concrete code examples
7. **Include rollback plans** — consumers need a safety net
8. **Pause for confirmation** — after Phase 1 analysis, stop and wait before writing

## Reference Files

When generating this guide, consult these project files if they exist:
- `CHANGELOG.md` — Version history with breaking changes and migration notes
- `README.md` — Existing migration notes (e.g., Vault Migration section)
- `docs/architecture/02-endpoint-catalog.md` — Public API surface for comparison
- `docs/architecture/07-configuration-analysis.md` — Configuration properties for comparison
- `docs/architecture/06-integration-map.md` — External service changes between versions
- `pom.xml` / `package.json` — Dependency versions for comparison
- `conf/` — Configuration files for property comparison
- `docs/prompts/release-versioning-sop-prompt.md` — Release process and versioning rules

## Organization Standards Reference

Standards and templates are maintained in your organization's standards repository (set its location for your team). Referenced files:
- `docs/prompts/readme-changelog-generator-prompt.md` — CHANGELOG format with breaking change documentation
- `docs/prompts/readme-changelog-best-practices.md` — Keep a Changelog standards for migration notes
```

---

## Usage Notes

### How to Use This Prompt

1. Open any project in your AI coding assistant
2. Copy the entire prompt above (everything between the outer triple backticks)
3. Paste it into the chat
4. Optionally add: "Focus on the migration from X.x to Y.0" to scope the work
5. The AI will analyze breaking changes, report findings, and wait for confirmation
6. After confirmation, it generates the migration guide

### Output Location

| Output | Suggested Location |
|---|---|
| Migration Guide | `docs/MIGRATION.md` or `docs/upgrade-guide.md` |

### Relationship to Other Prompts

- **Release SOP prompt** (`docs/prompts/release-versioning-sop-prompt.md`) — Defines when MAJOR versions happen; this documents how to migrate across them
- **README & Changelog prompt** (`docs/prompts/readme-changelog-generator-prompt.md`) — CHANGELOG contains the breaking change entries this guide expands on
- **This prompt** — Generates detailed migration instructions for consumers

---

**Last Updated**: 2026-04-23 (CST)
