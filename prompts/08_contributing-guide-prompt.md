# Contributing Guide Generator Prompt

> **Purpose**: Copy and paste this prompt into Kiro (or any AI coding assistant) to generate a CONTRIBUTING.md for any project. The AI will analyze the project's git workflow, CI/CD pipeline, code standards, and review process to produce a guide that keeps contributions consistent.
>
> **Standards**: Follows [GitHub's guide to setting guidelines for repository contributors](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors) and your organization's internal documentation standards.

---

## The Prompt

```
I need you to generate a comprehensive CONTRIBUTING.md for this project. This guide should define the rules and workflow for contributing code, documentation, and bug reports. Follow the process below systematically.

## Phase 1: Project Workflow Analysis (Do This First — Report Before Writing)

Analyze the project and report findings. I will confirm before you begin writing.

### 1.1 Git Workflow

Determine:
- **Default branch**: main, master, develop, or other
- **Branch naming convention**: From existing branches, CI config, or documentation
- **Merge strategy**: Merge commits, squash, rebase — check CI config and merge request settings
- **Protected branches**: Which branches require review or pipeline success
- **Tag/release strategy**: How versions are tagged and released

### 1.2 Commit Standards

Check for:
- **Conventional Commits**: commitlint config, commit message patterns in git log
- **Commit message format**: From documentation, git hooks, or CI validation
- **Issue/ticket references**: JIRA, GitLab, TargetProcess patterns in commit messages
- **Sign-off requirements**: DCO, GPG signing

### 1.3 Code Quality Gates

Identify:
- **Code formatting**: Spotless, Prettier, Black, gofmt — tool and config
- **Linting**: ESLint, Checkstyle, Pylint, Clippy — tool and config
- **Static analysis**: SonarQube, CodeClimate, Fortify
- **Test requirements**: Minimum coverage, required test types, test tags
- **Pre-commit hooks**: What runs before commit (formatting, linting, tests, secret scanning)

### 1.4 Review Process

Look for:
- **Code review requirements**: Number of approvals, required reviewers
- **Review checklist**: Security review, documentation review, test review
- **CI/CD gates**: What must pass before merge
- **Review tools**: GitLab MR, GitHub PR, Gerrit, Crucible

### 1.5 Issue/Ticket Workflow

Determine:
- **Issue tracker**: JIRA, GitLab Issues, GitHub Issues, TargetProcess
- **Issue types**: Bug, feature, task, story, epic
- **Issue templates**: Bug report template, feature request template
- **Ticket reference format**: How tickets are referenced in branches and commits

### 1.6 Documentation Requirements

Check for:
- **Documentation standards**: From steering files, style guides, or existing docs
- **Javadoc/docstring requirements**: For public APIs
- **README/CHANGELOG maintenance**: Who updates them and when
- **Architecture doc updates**: When architecture docs need updating

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Generate CONTRIBUTING.md

After confirmation, generate CONTRIBUTING.md with the following structure. Skip sections that don't apply.

### Document Structure

```markdown
# Contributing to [Project Name]

Thank you for your interest in contributing to [Project Name]. This guide explains the process and standards for contributing code, documentation, and bug reports.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Testing Requirements](#testing-requirements)
- [Documentation](#documentation)
- [Review Process](#review-process)
- [Release Process](#release-process)

---

## Code of Conduct

[Brief statement about expected behavior, or link to CODE_OF_CONDUCT.md if it exists.]

---

## Getting Started

Before contributing, ensure you have:
- [ ] Read the [README](README.md) for project overview
- [ ] Set up your development environment per the [Developer Guide](docs/onboarding/developer-setup.md)
- [ ] Access to the [issue tracker]([link])
- [ ] Familiarity with the [architecture documentation](docs/architecture/)

---

## How to Contribute

### Reporting Bugs

1. Check existing issues to avoid duplicates
2. Create a new issue with:
   - **Summary**: Clear, concise title
   - **Steps to reproduce**: Numbered steps to trigger the bug
   - **Expected behavior**: What should happen
   - **Actual behavior**: What actually happens
   - **Environment**: Version, OS, runtime details
3. Use the label `bug` (or equivalent)

### Requesting Features

1. Check existing issues and roadmap
2. Create a new issue with:
   - **Summary**: What you want and why
   - **Use case**: The problem this solves
   - **Proposed solution**: How you think it should work (optional)
3. Use the label `feature` or `enhancement`

### Contributing Code

1. Pick an issue or create one describing your change
2. Follow the [Development Workflow](#development-workflow) below
3. Submit a merge/pull request for review

---

## Development Workflow

### Branch Naming

All branches must follow this naming convention:

```
<type>/<ticket-id>-<short-description>
```

| Type | Use For | Example |
|------|---------|---------|
| `feature/` | New features | `feature/PROJ-123-add-user-auth` |
| `fix/` | Bug fixes | `fix/PROJ-456-null-pointer-login` |
| `docs/` | Documentation only | `docs/PROJ-789-update-api-docs` |
| `refactor/` | Code refactoring | `refactor/PROJ-101-extract-service` |
| `test/` | Test additions/fixes | `test/PROJ-202-add-unit-tests` |
| `chore/` | Maintenance tasks | `chore/PROJ-303-update-dependencies` |

### Commit Messages

All commits must follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`, `ci`, `build`

**Examples**:
```
feat(auth): add OAuth2 authentication support

Closes PROJ-123
```

```
fix(api): resolve null pointer in user lookup

The getUserById method did not handle missing users gracefully.
Previously it threw an unhandled NullPointerException.

Fixes PROJ-456
```

### Workflow Steps

1. **Create a branch** from `[default branch]`:
   ```bash
   git checkout [default-branch]
   git pull origin [default-branch]
   git checkout -b <type>/<ticket-id>-<description>
   ```

2. **Make your changes** following the [Coding Standards](#coding-standards)

3. **Run the pre-commit checklist**:
   - [ ] Code compiles without errors
   - [ ] All existing tests pass
   - [ ] New tests added for new functionality
   - [ ] Code formatted per project standards
   - [ ] No secrets or credentials in code
   - [ ] No TODO/FIXME without a ticket reference

4. **Commit and push**:
   ```bash
   git add [specific files]
   git commit -m "<type>(scope): description"
   git push -u origin <branch-name>
   ```

5. **Create a merge/pull request**:
   - Title: `<type>(scope): description` (matches commit convention)
   - Description: What changed, why, and how to test
   - Link the issue/ticket
   - Request review from the appropriate team members

---

## Coding Standards

### Code Style

[Reference the project's coding standards — language-specific style guide, formatting tool, and how to run it.]

### Security

All code must follow the security standards defined in the project's security guidelines:
- Validate all inputs
- Use parameterized queries for database operations
- No secrets in code (use environment variables or secrets management)
- Follow OWASP Top 10 guidelines
- Secure error handling (no stack traces in responses)

---

## Testing Requirements

### What to Test

- **New features**: Must include tests covering the happy path and key edge cases
- **Bug fixes**: Must include a test that reproduces the bug and verifies the fix
- **Refactoring**: Existing tests must continue to pass; add tests if coverage gaps are found

### Test Categories

[Reference the project's test categorization strategy — unit, integration, approval, e2e — and how to tag/run each.]

### Running Tests

```bash
# All tests
[test command]

# Unit tests only
[unit test command]

# Specific test class
[single test command]
```

---

## Documentation

### When to Update Documentation

- **New public API**: Add Javadoc/docstrings to all public classes and methods
- **New feature**: Update the README if it changes user-facing behavior
- **Architecture change**: Update the relevant doc in `docs/architecture/`
- **Breaking change**: Add migration notes to the CHANGELOG
- **Configuration change**: Update the configuration reference

### Documentation Standards

[Reference the project's documentation standards — markdown format, Javadoc requirements, etc.]

---

## Review Process

### What Reviewers Look For

1. **Correctness**: Does the code do what it claims?
2. **Security**: Are there any security vulnerabilities? (See security standards)
3. **Tests**: Are changes adequately tested?
4. **Style**: Does the code follow project conventions?
5. **Documentation**: Are public APIs documented?
6. **Performance**: Are there any obvious performance issues?

### Review Timeline

- Reviews should be completed within [X business days]
- If no review after [X days], ping the team in [channel]

### Merge Requirements

Before a merge/pull request can be merged:
- [ ] CI/CD pipeline passes
- [ ] Required number of approvals received
- [ ] No unresolved review comments
- [ ] Branch is up to date with `[default branch]`
- [ ] All conversations resolved

---

## Release Process

[Brief description of how releases work — who can release, versioning strategy, changelog updates.]

### Versioning

This project follows [Semantic Versioning](https://semver.org/):
- **MAJOR**: Breaking changes (incompatible API changes)
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes (backward compatible)

---

## Questions?

- **Team**: [Team name] — [contact]
- **Issue Tracker**: [Link]
- **Documentation**: [Link]
```

## Phase 3: Validation

After generating the guide, verify:

### Contributing Guide Validation
- [ ] Branch naming convention matches the project's actual practice
- [ ] Commit message format matches the project's actual convention
- [ ] Build and test commands are real and would work if copy-pasted
- [ ] Review process matches the project's actual workflow
- [ ] Security standards are referenced (not duplicated)
- [ ] Links to existing documentation (README, architecture docs, developer guide) are correct
- [ ] No secrets, passwords, or tokens appear anywhere
- [ ] Issue tracker references match the project's actual tracker
- [ ] CI/CD pipeline description matches the actual pipeline

## Rules

1. **Read the actual git history** — derive conventions from real branch names and commit messages
2. **Read CI/CD config** — understand what gates exist before documenting them
3. **Don't invent process** — only document workflows that actually exist or are explicitly requested
4. **Link to existing docs** — don't duplicate content from README, developer guide, or standards files
5. **Be specific** — "Run `mvn test`" not "Run the tests"
6. **Pause for confirmation** — after Phase 1 analysis, stop and wait before writing

## Reference Files

When generating this guide, consult these project files if they exist:
- `README.md` — Project overview and team contact
- `.gitlab-ci.yml` / `.github/workflows/` — CI/CD pipeline configuration
- `Makefile` — Build/test shortcuts
- `.kiro/steering/conventional-commits.md` — Commit message standards
- `.kiro/steering/java-standards.md` — Java coding standards
- `.kiro/steering/security-standards.md` — Security requirements
- `.kiro/steering/junit5-tag-strategy.md` — Test categorization
- `.kiro/skills/git-commit-standards/SKILL.md` — Branch naming, commit format
- `.kiro/skills/code-review/SKILL.md` — Code review workflow
- `.kiro/hooks/pre-commit-validation.kiro.hook` — Pre-commit checks
- `docs/prompts/developer-onboarding-guide-prompt.md` — Developer setup guide prompt

## Organization Standards Reference

Standards and templates are maintained in your organization's standards repository (set its location for your team). Referenced files:
- `steering/conventional-commits.md` — Conventional Commits specification
- `steering/java-standards.md` — Google Java Style Guide, package naming strategy
- `steering/security-standards.md` — OWASP/NIST security standards
- `skills/git-commit-standards/SKILL.md` — Branch naming, commit format, CI skip
- `skills/code-review/SKILL.md` — Security-focused code review workflow
```

---

## Usage Notes

### How to Use This Prompt

1. Open any project in your AI coding assistant
2. Copy the entire prompt above (everything between the outer triple backticks)
3. Paste it into the chat
4. The AI will analyze the project, report findings, and wait for confirmation
5. After confirmation, it generates CONTRIBUTING.md

### Output Location

| Output | Suggested Location |
|---|---|
| Contributing Guide | `CONTRIBUTING.md` (project root) |

### Relationship to Other Prompts

- **Developer Onboarding prompt** (`docs/prompts/developer-onboarding-guide-prompt.md`) — Generates the setup guide this references
- **README & Changelog prompt** (`docs/prompts/readme-changelog-generator-prompt.md`) — Generates the README that links to this guide
- **This prompt** — Generates the contribution workflow and standards guide

---

**Last Updated**: 2026-04-23 (CST)
