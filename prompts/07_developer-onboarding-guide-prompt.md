# Developer Onboarding Guide Generator Prompt

> **Purpose**: Copy and paste this prompt into Kiro (or any AI coding assistant) to generate a comprehensive developer onboarding and setup guide for any software project. The AI will analyze the project's build system, dependencies, configuration, and runtime requirements to produce a guide that gets a new developer from zero to productive.
>
> **Standards**: Follows [Make a README](https://www.makeareadme.com/) "Getting Started" best practices and your organization's internal documentation standards.

---

## The Prompt

```
I need you to generate a comprehensive Developer Onboarding Guide for this project. This guide should take a new developer from zero knowledge to a working local development environment and first successful contribution. Follow the process below systematically.

## Phase 1: Environment Analysis (Do This First — Report Before Writing)

Analyze the project and report findings. I will confirm before you begin writing.

### 1.1 Build System Detection

Determine:
- **Build tool**: Maven, Gradle, npm, yarn, pip, Cargo, Make, CMake, etc.
- **Build commands**: From the build manifest (pom.xml, package.json, Makefile, etc.)
- **Build profiles**: Any environment-specific profiles (dev, test, prod, CI)
- **Build plugins**: Code formatting, static analysis, code generation, etc.
- **Artifact type**: JAR, WAR, EAR, Docker image, npm package, binary, etc.

### 1.2 Runtime Requirements

Scan for:
- **Language version**: Check compiler settings, toolchain files, CI config, or manifest
- **Runtime/server**: Application server (JBoss, Tomcat, Jetty), Node.js, Python runtime, etc.
- **System properties**: JVM args, environment variables, system properties required at runtime
- **External tools**: CLI tools invoked by the code (virus scanners, compilers, linters, etc.)
- **Docker requirements**: Dockerfile, docker-compose, TestContainers usage

### 1.3 Configuration Analysis

Identify:
- **Config files**: INI, YAML, properties, JSON, .env files
- **Environment selection**: How the app determines which environment config to use
- **Local development overrides**: System properties, env vars, or config files for local dev
- **Secrets management**: Vault, AWS Secrets Manager, .env files, keystores
- **Local development fallbacks**: What happens when external services (Vault, LDAP, etc.) are unavailable locally

### 1.4 Dependency Analysis

Check for:
- **Internal/organizational dependencies**: Libraries from the same org that may require special repository access
- **Repository configuration**: Private Maven repos, npm registries, PyPI mirrors
- **Authentication for dependencies**: Tokens or credentials needed to pull internal packages
- **Transitive dependency issues**: Known conflicts, exclusions, or version pinning

### 1.5 Test Infrastructure

Determine:
- **Test framework**: JUnit, Jest, pytest, Go test, etc.
- **Test categories/tags**: Unit, integration, approval, e2e — and how to run each
- **Test dependencies**: TestContainers (Docker), embedded databases, mock servers
- **Test data**: Fixtures, seed data, approval files, golden files
- **Test commands**: How to run all tests, specific categories, single tests

### 1.6 IDE and Tooling

Look for:
- **IDE configuration**: .idea/, .vscode/, .editorconfig, .prettierrc
- **Code formatting**: Spotless, Prettier, Black, gofmt — and how to run it
- **Pre-commit hooks**: Husky, pre-commit framework, custom git hooks
- **Linting**: ESLint, Checkstyle, Pylint, Clippy
- **Recommended extensions**: From .vscode/extensions.json or documentation

### 1.7 Access Requirements

Identify:
- **Repository access**: GitLab, GitHub, Bitbucket — any group/org membership needed
- **CI/CD access**: Pipeline visibility, deployment permissions
- **External service access**: VPN, LDAP, Vault, database access
- **Issue tracker**: JIRA, GitLab Issues, TargetProcess — and how to reference tickets

### 1.8 AI-Assisted Development Environment (Kiro / MCP / CLI)

Determine what AI-assisted development tooling the project uses or should use:

**Kiro IDE Setup:**
- Is Kiro IDE installed? (Mac installation)
- Does Kiro require AWS authentication? What's the SSO Start URL? (e.g., `https://<your-org>.awsapps.com/start`)
- Is a Kiro license needed? What's the request process? (e.g., a security/access request ticket — reference existing ticket numbers if available)

**SSH and Git Authentication:**
- SSH key generated and added to GitLab account ([GitLab SSH docs](https://docs.gitlab.com/user/ssh/))
- Repos cloned via SSH

**GitLab CLI (`glab`) Setup:**
- Is `glab` installed?
- GitLab Personal Access Token (PAT) created ([GitLab PAT docs](https://docs.gitlab.com/user/profile/personal_access_tokens/))
- Environment variables configured in shell profile (`~/.zshrc` or `~/.bashrc`):
  - `GITLAB_TOKEN` — PAT for glab authentication
  - `GITLAB_HOST` — GitLab hostname (e.g., `gitlab.com`)
  - Any org-specific variables (e.g., `ORG_GITLAB_SUBGROUP_ID`, `ORG_GITLAB_SUBGROUP_PATH`)
- Verification: `source ~/.zshrc && glab auth status`

**MCP Server Configuration:**
- What Model Context Protocol servers are configured for the project? Check `.kiro/settings/mcp.json` and `~/.kiro/settings/mcp.json`
  - **Dynatrace MCP Server** — observability: problems, logs, metrics, vulnerabilities for project services
  - **Chrome DevTools MCP Server** — frontend debugging: DOM inspection, network monitoring, console, performance tracing
  - Other project-specific MCP servers
- Is `uv` / `uvx` installed? (required for running MCP servers)
- Are MCP server connections verified and working?

**Kiro Workspace Configuration:**
- Does `.kiro/steering/` exist with project-specific steering rules?
- Does `.kiro/hooks/` exist with project-specific automation hooks?
- Does `.kiro/skills/` exist with reusable AI instructions?
- Are there personas configured for the team?

**Observability and Monitoring Access:**
- Dynatrace environment access — can the developer log in and see project services?
- Dynatrace API token generated with appropriate scopes (for MCP server)

**Local Build and Runtime Verification (AI-Assisted Development Readiness):**
- All project applications compile, build, and run locally
- AI can see compile output, logs, and `#Problems` / `#Terminal` context in Kiro
- Chrome DevTools MCP can connect to locally running frontend application (if applicable)

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Generate the Onboarding Guide

After confirmation, generate the guide with the following structure. Skip sections that don't apply.

### Document Structure

```markdown
# Developer Onboarding Guide — [Project Name]

## Overview

[1-2 sentences: what this project is, what it does, and who consumes it. Link to the main README for full details.]

**Prerequisites reading**: Before setting up your environment, read the [README](../../README.md) for project context and the [Architecture Documentation](../../docs/architecture/) for system design.

---

## Prerequisites

### Required Software

| Tool | Version | Installation | Verification |
|------|---------|-------------|--------------|
| [Language] | [version] | [install command or link] | `[verify command]` |
| [Build tool] | [version] | [install command or link] | `[verify command]` |
| [Runtime] | [version] | [install command or link] | `[verify command]` |
| [Other tools] | [version] | [install command or link] | `[verify command]` |

### Access Requirements

Before you begin, ensure you have:
- [ ] Repository access ([platform] — request from [team/admin])
- [ ] [Internal package registry] access (for organizational dependencies)
- [ ] [VPN/network] access (if required for external services)
- [ ] [Issue tracker] account ([platform] — [project/board])
- [ ] [CI/CD] pipeline visibility

---

## Initial Setup

### 1. Clone the Repository

```bash
git clone [repository-url]
cd [project-name]
```

### 2. Configure Build Tool

[Repository settings, authentication for private registries, proxy configuration, etc.]

### 3. Build the Project

```bash
[exact build command]
```

**Expected output**: [What a successful build looks like — artifact name, location, approximate time]

**Common build failures**:
- [Failure 1]: [Cause and fix]
- [Failure 2]: [Cause and fix]

### 4. Run Tests

```bash
# Run all tests
[test command]

# Run only unit tests (fast feedback)
[unit test command]

# Run integration tests (may require Docker or external services)
[integration test command]
```

**Expected output**: [Number of tests, approximate time, what success looks like]

---

## Local Development Configuration

### Environment Configuration

[How to configure the project for local development — config files, system properties, environment variables]

### Local Development Shortcuts

[Fallbacks for external services that aren't available locally — mock modes, local user overrides, embedded alternatives]

### IDE Setup

#### [Primary IDE]

1. [Import/open instructions]
2. [Code style configuration]
3. [Run configuration setup]
4. [Recommended plugins/extensions]

#### Code Formatting

[How to format code to match project standards — command, IDE integration, pre-commit hook]

---

## AI-Assisted Development Environment Setup

### Kiro IDE

| Step | Action | Verification |
|------|--------|-------------|
| Install Kiro | [Install Kiro IDE on Mac] | Kiro launches successfully |
| AWS Authentication | Kiro will prompt for the Start URL: `[SSO URL]` | Kiro authenticated with AWS |
| License (if needed) | [License request process — e.g., submit an access/license request ticket at [ticketing system]] | License active in Kiro |

### SSH Key Setup

1. Generate an SSH key if you don't have one:
   ```bash
   ssh-keygen -t ed25519 -C "[your-email]"
   ```
2. Add the public key to your GitLab account: [GitLab SSH docs](https://docs.gitlab.com/user/ssh/)
3. Verify: `ssh -T git@gitlab.com`

### GitLab CLI (`glab`)

1. Install `glab`:
   ```bash
   brew install glab
   ```

2. Create a [GitLab Personal Access Token (PAT)](https://docs.gitlab.com/user/profile/personal_access_tokens/) with appropriate scopes

3. Add environment variables to `~/.zshrc` (or `~/.bashrc`):
   ```bash
   export GITLAB_TOKEN="<your-pat-here>"
   export GITLAB_HOST="gitlab.com"
   # Add any org-specific variables:
   # export ORG_GITLAB_SUBGROUP_ID="<subgroup-id>"
   # export ORG_GITLAB_SUBGROUP_PATH="<subgroup-path>"
   ```

4. Apply and verify:
   ```bash
   source ~/.zshrc
   glab auth status
   ```

### MCP Server Configuration

Ensure MCP servers are configured in `.kiro/settings/mcp.json` or `~/.kiro/settings/mcp.json`:

**Dynatrace MCP Server** (observability):
- Verify Dynatrace environment access — log in and confirm appropriate roles
- Generate a Dynatrace API token with required scopes
- Test: query active problems or logs for project services via Kiro

**Chrome DevTools MCP Server** (frontend debugging — if applicable):
- Ensure Google Chrome is installed
- Start the frontend application locally
- Test: take a page snapshot and inspect DOM via Kiro

**Other MCP servers**: [List any project-specific MCP servers]

Install `uv` / `uvx` if not already available (required for running MCP servers):
```bash
brew install uv
```

### Kiro Workspace Verification

After setup, verify the AI-assisted development environment is fully working:
- [ ] Kiro IDE running and authenticated
- [ ] All project applications compile, build, and run locally
- [ ] AI can see compile output, logs, and `#Problems` / `#Terminal` context
- [ ] MCP servers connected and responding
- [ ] GitLab CLI authenticated (`glab auth status`)
- [ ] `.kiro/steering/` rules loaded (if they exist for the project)
- [ ] `.kiro/hooks/` automation active (if configured)

---

## Project Structure

[Brief tour of the directory layout — what lives where, where to find things]

```
project-root/
├── src/main/          # [Description]
├── src/test/          # [Description]
├── conf/              # [Description]
├── docs/              # [Description]
│   └── architecture/  # [Description]
├── scripts/           # [Description]
└── [build manifest]   # [Description]
```

---

## Development Workflow

### Branch Naming

[Branch naming convention — reference the project's commit/branch standards]

### Making Changes

1. Create a branch from `[default branch]`
2. Make your changes
3. Run tests locally: `[test command]`
4. Format code: `[format command]`
5. Commit using [commit convention]: `[example commit]`
6. Push and create a merge/pull request

### Commit Message Format

[Reference the project's conventional commit standards or commit message format]

### Code Review Process

[How code reviews work — who reviews, what's expected, how to request review]

### CI/CD Pipeline

[What happens when you push — pipeline stages, expected duration, how to check status]

---

## Key Concepts for New Developers

### Architecture Overview

[2-3 paragraph summary of the architecture — link to full architecture docs for details]

### Important Patterns

[List the 3-5 most important code patterns a new developer needs to understand. Link to the code organization/patterns doc if it exists.]

### Common Pitfalls

[Things that trip up new developers — gotchas, non-obvious behaviors, legacy constraints]

---

## Troubleshooting

### Build Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| [Symptom] | [Why it happens] | [How to fix] |

### Test Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| [Symptom] | [Why it happens] | [How to fix] |

### Runtime Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| [Symptom] | [Why it happens] | [How to fix] |

---

## Getting Help

- **Team**: [Team name] — [email or chat channel]
- **Issue Tracker**: [Link]
- **Documentation**: [Link to docs/]
- **Architecture Docs**: [Link to docs/architecture/]

---

*Last Updated: [DATE]*
```

## Phase 3: Validation

After generating the guide, verify:

### Onboarding Guide Validation
- [ ] All build commands are real and would work if copy-pasted
- [ ] All prerequisite versions match the project's actual requirements
- [ ] Verification commands are included for every prerequisite
- [ ] Local development configuration is complete (a new dev could follow it end-to-end)
- [ ] Common build/test failures are documented with solutions
- [ ] No secrets, passwords, or tokens appear anywhere (use placeholders)
- [ ] Links to existing documentation (README, architecture docs) are correct
- [ ] IDE setup instructions are specific and actionable
- [ ] Commit message format matches the project's actual convention
- [ ] Troubleshooting section covers the most likely issues a new developer would hit
- [ ] AI-assisted development environment section is complete (Kiro, MCP servers, GitLab CLI)
- [ ] MCP server configuration is documented with verification steps
- [ ] GitLab CLI setup includes PAT creation and environment variable configuration
- [ ] Kiro workspace verification checklist covers all AI-assisted development prerequisites

## Rules

1. **Read the actual build manifest** — use real commands, real versions, real artifact names
2. **Test the commands mentally** — would they work if copy-pasted into a fresh terminal?
3. **Don't invent requirements** — only list prerequisites that the project actually needs
4. **Link to existing docs** — don't duplicate content from README or architecture docs; reference them
5. **No secrets** — use placeholders for any credentials, tokens, or connection strings
6. **Be specific about versions** — "Java 8" not "Java", "Maven 3.9+" not "Maven"
7. **Include failure scenarios** — new developers will hit problems; document the common ones
8. **Pause for confirmation** — after Phase 1 analysis, stop and wait before writing

## Reference Files

When generating this guide, consult these project files if they exist:
- `README.md` — Project overview, tech stack, architecture doc links
- `pom.xml` / `package.json` / `Cargo.toml` — Build manifest with dependencies and versions
- `docs/architecture/` — Architecture documentation for the "Key Concepts" section
- `conf/` — Configuration files for the "Local Development Configuration" section
- `.gitlab-ci.yml` / `.github/workflows/` — CI/CD config for the "Pipeline" section
- `.kiro/steering/java-standards.md` — Java coding standards (if Java project)
- `.kiro/steering/conventional-commits.md` — Commit message format
- `.kiro/steering/junit5-tag-strategy.md` — Test categorization strategy (if JUnit project)
- `.kiro/settings/mcp.json` — MCP server configuration for the project
- `~/.kiro/settings/mcp.json` — User-level MCP server configuration
- `Makefile` — Build/test shortcuts
- `.editorconfig` / `.prettierrc` / `spotless` config — Code formatting rules

## AI-Assisted Development Training Reference

If your organization maintains an AI-assisted development training plan, reference its pre-work / developer-setup checklist here.

That pre-work checklist typically defines the developer setup steps for AI-assisted development with Kiro, including:
- Kiro IDE installation and AWS authentication
- SSH key setup for GitLab
- GitLab CLI (`glab`) installation and PAT configuration
- MCP server setup (Dynatrace, Chrome DevTools)
- Environment variable configuration (`GITLAB_TOKEN`, `GITLAB_HOST`, org-specific vars)
- Local build and runtime verification for AI readiness

Use this as a reference when generating the AI-Assisted Development Environment Setup section of the onboarding guide.

## Organization Standards Reference

Standards and templates are maintained in your organization's standards repository (set its location for your team). Referenced files:
- `steering/java-standards.md` — Google Java Style Guide, package naming strategy
- `steering/conventional-commits.md` — Conventional Commits specification
- `steering/junit5-tag-strategy.md` — JUnit 5 tag-based test categorization
- `steering/security-standards.md` — OWASP/NIST security standards
- `skills/git-commit-standards/SKILL.md` — Branch naming, commit format, CI skip
```

---

## Usage Notes

### How to Use This Prompt

1. Open any project in your AI coding assistant
2. Copy the entire prompt above (everything between the outer triple backticks)
3. Paste it into the chat
4. The AI will analyze the project, report findings, and wait for confirmation
5. After confirmation, it generates the onboarding guide

### Output Location

| Output | Suggested Location |
|---|---|
| Developer Onboarding Guide | `docs/onboarding/developer-setup.md` or `docs/DEVELOPER_GUIDE.md` |

### Relationship to Other Prompts

- **README & Changelog prompt** (`docs/prompts/readme-changelog-generator-prompt.md`) — Generates the README that this guide references
- **This prompt** — Generates the detailed setup guide that the README's "Getting Started" section links to
- **Contributing Guide prompt** (`docs/prompts/contributing-guide-prompt.md`) — Generates the contribution workflow that this guide's "Development Workflow" section references

---

**Last Updated**: 2026-04-23 (CST)
