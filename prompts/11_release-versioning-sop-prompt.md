# Release and Versioning SOP Generator Prompt

> **Purpose**: Copy and paste this prompt into Kiro (or any AI coding assistant) to generate a standard operating procedure for releasing new versions of any software project. The AI will analyze the project's build system, versioning strategy, CI/CD pipeline, and dependency relationships to produce a step-by-step release process.
>
> **Standards**: Follows [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html), [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and your organization's internal documentation standards.

---

## The Prompt

```
I need you to generate a Release and Versioning Standard Operating Procedure (SOP) for this project. This document should provide a repeatable, step-by-step process for releasing new versions — from deciding the version number through deployment and post-release verification. Follow the process below systematically.

## Phase 1: Release Process Analysis (Do This First — Report Before Writing)

Analyze the project and report findings. I will confirm before you begin writing.

### 1.1 Versioning Strategy

Determine:
- **Current version**: From build manifest (pom.xml, package.json, etc.)
- **Version history**: From git tags and CHANGELOG.md
- **Versioning scheme**: Semantic Versioning, CalVer, or custom
- **Version location**: Where the version is defined (manifest, config, multiple places)
- **SNAPSHOT/pre-release**: Whether the project uses SNAPSHOT versions or pre-release identifiers

### 1.2 Build and Artifact

Identify:
- **Build command**: How to build a release artifact
- **Artifact type**: JAR, WAR, Docker image, npm package, etc.
- **Artifact repository**: Where artifacts are published (Maven Central, npm registry, GitLab Package Registry, Nexus, etc.)
- **Artifact naming**: How the artifact is named and versioned

### 1.3 CI/CD Pipeline

Analyze:
- **Pipeline config**: What stages exist (build, test, publish, deploy)
- **Release triggers**: What triggers a release build (tag push, manual, merge to main)
- **Automated publishing**: Does the pipeline publish artifacts automatically?
- **Environment promotion**: How artifacts move from dev to QA to prod

### 1.4 Downstream Consumers

Identify:
- **Consumer projects**: What projects depend on this one
- **Dependency declaration**: How consumers reference this project (version range, exact version)
- **Breaking change impact**: What happens to consumers when a breaking change is released
- **Consumer update process**: How consumers adopt new versions

### 1.5 Release History Patterns

From git history and CHANGELOG:
- **Release frequency**: How often releases happen
- **Branch strategy**: Release branches, hotfix branches, or release from main
- **Tag format**: v1.0.0, 1.0.0, project-name-1.0.0, etc.
- **Changelog maintenance**: How the changelog is updated (manual, automated, per-release)

### 1.6 Approval and Governance

Check for:
- **RFC/change request process**: Is an RFC required for production releases?
- **Approval gates**: Who approves releases?
- **Release notes**: Where release notes are published (GitLab releases, CHANGELOG, wiki)
- **Communication**: How releases are communicated to consumers and stakeholders

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Generate the Release SOP

After confirmation, generate the SOP with the following structure. Skip sections that don't apply.

### Document Structure

```markdown
# Release and Versioning SOP — [Project Name]

## Overview

This document defines the standard operating procedure for releasing new versions of [Project Name]. It covers version numbering, the release process, and post-release verification.

**Versioning**: This project follows [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html).

---

## Version Numbering

### Semantic Versioning Rules

| Version Component | When to Increment | Example |
|-------------------|-------------------|---------|
| **MAJOR** (X.0.0) | Breaking changes — incompatible API changes, removed features, config property removal | 4.0.0 → 5.0.0 |
| **MINOR** (0.X.0) | New features — backward-compatible additions, new config properties, new public methods | 4.1.0 → 4.2.0 |
| **PATCH** (0.0.X) | Bug fixes — backward-compatible fixes, dependency updates, documentation fixes | 4.2.0 → 4.2.1 |

### How to Decide the Version

Ask these questions in order:

1. **Did you remove or rename a public method, class, or config property?** → MAJOR
2. **Did you change the behavior of an existing public method in a way that could break consumers?** → MAJOR
3. **Did you add a new public method, class, or config property?** → MINOR
4. **Did you fix a bug, update a dependency, or improve documentation?** → PATCH

### Breaking Change Checklist

Before releasing a MAJOR version, verify:
- [ ] All breaking changes are documented in CHANGELOG.md with migration instructions
- [ ] Consumer projects have been notified
- [ ] A migration path exists (ideally: deprecate in MINOR, remove in MAJOR)
- [ ] The previous version remains available for consumers who haven't migrated

---

## Pre-Release Checklist

Before starting the release process:

- [ ] All changes for this release are merged to `[default branch]`
- [ ] CI/CD pipeline is green on `[default branch]`
- [ ] All tests pass (unit, integration, approval)
- [ ] CHANGELOG.md is updated with the new version entry
- [ ] Version number is decided per the rules above
- [ ] No unresolved security vulnerabilities (check SonarQube/Fortify)
- [ ] Breaking changes (if any) have migration instructions in CHANGELOG
- [ ] Consumer teams notified (for MAJOR/MINOR releases)

---

## Release Process

### Step 1: Update Version

Update the version in the build manifest:

```bash
# [Exact command to update version — adapt per build tool]
# Maven example:
mvn versions:set -DnewVersion=X.Y.Z
mvn versions:commit

# npm example:
npm version X.Y.Z --no-git-tag-version

# Manual: Edit [manifest file] and change version to X.Y.Z
```

**Files to update**:
- [ ] `[build manifest]` — version field
- [ ] `CHANGELOG.md` — rename [Unreleased] to [X.Y.Z] with today's date
- [ ] Any other files that contain the version number

### Step 2: Update CHANGELOG

Move the [Unreleased] section contents to a new version entry:

```markdown
## [Unreleased]

(empty — ready for next development cycle)

---

## [X.Y.Z] — YYYY-MM-DD

### Added
- [entries moved from Unreleased]

### Changed
- [entries moved from Unreleased]
```

Add a version comparison link at the bottom of CHANGELOG.md:
```markdown
[X.Y.Z]: https://[platform]/compare/PREVIOUS...X.Y.Z
```

### Step 3: Commit and Tag

```bash
# Stage the version changes
git add [manifest file] CHANGELOG.md [other version files]

# Commit with conventional commit format
git commit -m "chore(release): release version X.Y.Z"

# Create an annotated tag
git tag -a vX.Y.Z -m "Release X.Y.Z"

# Push commit and tag
git push origin [default-branch]
git push origin vX.Y.Z
```

### Step 4: Verify Pipeline

1. Confirm the CI/CD pipeline triggers on the tag push
2. Verify all pipeline stages pass (build, test, publish)
3. Verify the artifact is published to the artifact repository

```bash
# [Commands to verify the artifact was published]
[verification command]
```

### Step 5: Create Release Notes (Optional)

If the project uses GitLab/GitHub releases:

```bash
# GitLab
glab release create vX.Y.Z --notes "$(cat CHANGELOG_EXCERPT.md)"

# GitHub
gh release create vX.Y.Z --notes "$(cat CHANGELOG_EXCERPT.md)"
```

### Step 6: Post-Release

- [ ] Verify artifact is available in the repository
- [ ] Notify consumer teams of the new release
- [ ] Update any documentation that references the version
- [ ] For MAJOR releases: confirm migration guides are accessible

---

## Hotfix Release Process

**When to use**: Critical bug in production that can't wait for the next planned release.

1. **Create hotfix branch** from the release tag:
   ```bash
   git checkout -b hotfix/X.Y.Z+1 vX.Y.Z
   ```

2. **Apply the fix** with tests

3. **Follow Steps 1-6** above with the patch version incremented

4. **Merge back** to `[default branch]`:
   ```bash
   git checkout [default-branch]
   git merge hotfix/X.Y.Z+1
   git push origin [default-branch]
   ```

5. **Delete the hotfix branch**

---

## Consumer Update Guide

### For Downstream Projects

When a new version of [Project Name] is released:

1. **Read the CHANGELOG** for the new version — check for breaking changes
2. **Update the dependency version** in your build manifest:
   ```xml
   <!-- Maven example -->
   <dependency>
       <groupId>[group]</groupId>
       <artifactId>[artifact]</artifactId>
       <version>X.Y.Z</version>
   </dependency>
   ```
3. **Build and test** your project
4. **Follow migration instructions** if upgrading across a MAJOR version

### Breaking Change Migration Pattern

For MAJOR version upgrades, follow this pattern:
1. First upgrade to the last MINOR version before the MAJOR (to get deprecation warnings)
2. Address all deprecation warnings
3. Then upgrade to the new MAJOR version

---

## Version History Quick Reference

[Link to CHANGELOG.md for full history]

---

*Last Updated: [DATE]*
```

## Phase 3: Validation

After generating the SOP, verify:

### Release SOP Validation
- [ ] Version numbering rules are clear and match Semantic Versioning
- [ ] Pre-release checklist is complete and actionable
- [ ] Every release step has exact commands that would work if copy-pasted
- [ ] Tag format matches the project's actual tag convention
- [ ] CI/CD pipeline trigger matches the actual pipeline config
- [ ] Artifact repository matches the project's actual publishing target
- [ ] Hotfix process is documented
- [ ] Consumer update guide is included (for libraries)
- [ ] CHANGELOG update instructions reference Keep a Changelog format
- [ ] No secrets, passwords, or tokens appear anywhere
- [ ] Links to existing documentation (CHANGELOG, README) are correct

## Rules

1. **Read the build manifest** — use real group IDs, artifact IDs, and version patterns
2. **Read the git tags** — derive the tag format from actual tags, not assumptions
3. **Read the CI/CD config** — understand what triggers releases and what stages run
4. **Read the CHANGELOG** — understand the existing changelog format and conventions
5. **Don't invent process** — only document what exists or is explicitly requested
6. **Be specific about commands** — exact commands, not "update the version"
7. **Include consumer guidance** — especially for libraries consumed by other projects
8. **Pause for confirmation** — after Phase 1 analysis, stop and wait before writing

## Reference Files

When generating this SOP, consult these project files if they exist:
- `pom.xml` / `package.json` / `Cargo.toml` — Build manifest with version and artifact coordinates
- `CHANGELOG.md` — Existing changelog format and version history
- `README.md` — Project overview, consumer projects, migration notes
- `.gitlab-ci.yml` / `.github/workflows/` — CI/CD pipeline configuration
- `Makefile` — Build and release shortcuts
- `scripts/` — Release scripts or version bump scripts
- `.kiro/steering/conventional-commits.md` — Commit message format for release commits
- `.kiro/skills/git-commit-standards/SKILL.md` — Branch naming and tag conventions
- `docs/prompts/readme-changelog-generator-prompt.md` — CHANGELOG format standards

## Organization Standards Reference

Standards and templates are maintained in your organization's standards repository (set its location for your team). Referenced files:
- `steering/conventional-commits.md` — Conventional Commits for release commit messages
- `skills/git-commit-standards/SKILL.md` — Branch naming, tag format, CI skip directives
- `skills/glab-cli-operations/SKILL.md` — GitLab CLI for release and pipeline operations
- `docs/prompts/readme-changelog-generator-prompt.md` — CHANGELOG format and best practices
- `docs/prompts/readme-changelog-best-practices.md` — Keep a Changelog and SemVer standards
```

---

## Usage Notes

### How to Use This Prompt

1. Open any project in your AI coding assistant
2. Copy the entire prompt above (everything between the outer triple backticks)
3. Paste it into the chat
4. The AI will analyze the release process, report findings, and wait for confirmation
5. After confirmation, it generates the release SOP

### Output Location

| Output | Suggested Location |
|---|---|
| Release SOP | `docs/operations/release-process.md` or `docs/RELEASE.md` |

### Relationship to Other Prompts

- **README & Changelog prompt** (`docs/prompts/readme-changelog-generator-prompt.md`) — Defines the CHANGELOG format this SOP maintains
- **Runbook prompt** (`docs/prompts/runbook-prompt.md`) — Runbooks cover deployment; this covers the release decision and preparation
- **Contributing Guide prompt** (`docs/prompts/contributing-guide-prompt.md`) — Contributing guide references the release process
- **This prompt** — Generates the version numbering and release procedure

---

**Last Updated**: 2026-04-23 (CST)
