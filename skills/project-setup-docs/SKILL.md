---
name: "project-setup-docs"
description: "Bootstrap a project's Kiro environment and generate the documentation suite (README, changelog, requirements, architecture, onboarding, contributing, tests, CI/CD, release SOP, runbook, troubleshooting, migration). Discovers the project, builds a tailored plan that runs only the prompts the project actually needs, then executes each in order."
metadata:
  version: "1.0.0"
  lastUpdated: "2026-06-10"
  lastUpdatedBy: "AI Assistant"
keywords:
  - project setup
  - project set-up
  - bootstrap
  - scaffold
  - reverse engineer
  - architecture docs
  - readme
  - changelog
  - onboarding
  - contributing
  - test strategy
  - cicd documentation
  - release sop
  - runbook
  - troubleshooting
  - migration guide
  - documentation suite
---

# Project Setup & Documentation Skill

Bootstrap a project's Kiro environment and generate its documentation suite by orchestrating the project setup prompts. Discovers the project first, builds a plan that runs only the prompts the project actually needs, confirms with the user, then executes each prompt in dependency order.

## Prompts Library Location

The prompts library ships alongside this skill. Look for the numbered setup prompts (`01_*.md` through `14_*.md`) in these locations, in order:

1. `prompts/` in this skill's own package (the directory that ships with this skill)
2. `prompts/project_set-up/` relative to the current workspace
3. `prompts/` relative to the current workspace

If none are available, ask the user where the prompts library lives before proceeding.

## When to Use

Activate this skill when the user mentions:
- Project set-up, project setup, set up the project
- Bootstrap a project, scaffold project docs
- Generate documentation for this project
- Run the project setup prompts
- Reverse-engineer and document this project
- Stand up a new project's documentation
- Onboard this project to Kiro standards

## Prerequisites

- The target project workspace must be open
- The prompts library must be accessible (see locations above)
- For bootstrap (prompt 01), access to your organization's standards repository (the "ai-rules" repo) is recommended

## Available Prompts

The skill orchestrates these 13 runnable prompts plus 1 reference document:

| # | Prompt | File | Output |
|---|--------|------|--------|
| 01 | Kiro Project Bootstrap | `01_kiro-project-bootstrap-prompt.md` | `.kiro/` steering, hooks, MCP, skills |
| 02 | Reverse Engineering | `02_reverse-engineering-kickoff-prompt.md` | `docs/architecture/01-10*.md`, steering file |
| 03 | Requirements Documentation | `03_requirements-documentation-prompt.md` | `docs/requirements/requirements.md` |
| 04 | README/Changelog Best Practices | `04_readme-changelog-best-practices.md` | Reference only — not run |
| 05 | README & Changelog Generator | `05_readme-changelog-generator-prompt.md` | `README.md`, `CHANGELOG.md` |
| 06 | Javadoc Generation | `06_javadoc-generation-prompt.md` | Inline doc comments in source |
| 07 | Developer Onboarding Guide | `07_developer-onboarding-guide-prompt.md` | `docs/onboarding/developer-setup.md` |
| 08 | Contributing Guide | `08_contributing-guide-prompt.md` | `CONTRIBUTING.md` |
| 09 | Test Strategy | `09_test-strategy-prompt.md` | `docs/testing/test-strategy.md` |
| 10 | CI/CD Pipeline Documentation | `10_cicd-pipeline-documentation-prompt.md` | `docs/cicd/pipeline.md` |
| 11 | Release & Versioning SOP | `11_release-versioning-sop-prompt.md` | `docs/operations/release-process.md` |
| 12 | Runbook | `12_runbook-prompt.md` | `docs/operations/runbook.md` |
| 13 | Troubleshooting Guide | `13_troubleshooting-guide-prompt.md` | `docs/troubleshooting.md` |
| 14 | Migration & Upgrade Guide | `14_migration-upgrade-guide-prompt.md` | `docs/MIGRATION.md` |

## Full Workflow

### Phase 1: Discover the Project

Read the project thoroughly. Do NOT skip files or summarize from filenames alone — every downstream decision depends on the depth of this discovery.

1. **Project identity**
   - Read `README.md`, dependency manifests (`pom.xml`, `package.json`, `Cargo.toml`, `go.mod`, `requirements.txt`, `pyproject.toml`, `CMakeLists.txt`, `*.csproj`, etc.)
   - Capture: project name, description, current version, license, repository URL, team/owner

2. **Tech stack**
   - Language(s) and version
   - Framework (Spring Boot, Angular, React, Django, Express, etc.)
   - Build system (Maven, Gradle, npm, yarn, pip, Cargo, Make, CMake, ng)
   - Test framework (JUnit, Jest, pytest, Go test, Catch2, Karma, Vitest)

3. **Project type** — pick one (this drives most decisions)
   - **Library/SDK** — consumed as a dependency by other projects
   - **Application** — standalone executable (CLI, batch, daemon)
   - **Service/API** — exposes endpoints (REST, gRPC, GraphQL)
   - **UI application** — visual interface (Angular, React, Vue, mobile)
   - **Infrastructure/IaC** — Helm, Terraform, Ansible, pipeline templates
   - **Monorepo** — multiple packages with independent versioning
   - **Documentation/prompts repo** — primarily markdown, no compiled code

4. **Existing assets** (preserve these — never overwrite without confirmation)
   - `.kiro/` directory contents (steering, hooks, MCP config, skills, specs)
   - `docs/architecture/`, `docs/onboarding/`, `docs/operations/`, `docs/testing/`, `docs/cicd/`
   - `README.md`, `CHANGELOG.md`, `CONTRIBUTING.md`, `MIGRATION.md`, `TROUBLESHOOTING.md`, `LICENSE`
   - Any `docs/requirements/` content

5. **Capability signals** (these gate which prompts run)
   - **Has source code?** Any production source files exist
   - **Has public API?** Library/SDK with consumer-facing classes/exports
   - **Has tests?** `src/test/`, `__tests__/`, `tests/`, `*_test.go`, `*.test.ts`, etc.
   - **Has CI/CD?** `.gitlab-ci.yml`, `.github/workflows/`, `Jenkinsfile`, `azure-pipelines.yml`
   - **Has deployment artifacts?** `Dockerfile`, `docker-compose.yml`, `helm/`, `deploy/`, `k8s/`, `terraform/`
   - **Has external integrations?** HTTP clients, JDBC drivers, MQ clients, external CLI invocations
   - **Has a database?** JDBC/ORM dependencies, migration files, connection strings in config
   - **Has version history?** Git tags exist, `CHANGELOG.md` populated
   - **Has multiple major versions?** Two or more distinct major versions in tags/changelog
   - **Has Java public API?** Java project with `public class`/`public interface` in `src/main/`

6. **Report findings** — present a one-screen summary to the user before Phase 2.

### Phase 2: Build the Tailored Plan

Apply the decision matrix below to select which prompts apply. Output a numbered execution plan.

#### Decision Matrix

| # | Prompt | Run when... | Skip when... |
|---|--------|-------------|--------------|
| 01 | Bootstrap | `.kiro/steering/` is empty or missing | `.kiro/steering/` is already populated and user did not ask to refresh |
| 02 | Reverse Engineering | Source code exists AND no `docs/architecture/` directory | `docs/architecture/01-10*.md` already exist |
| 03 | Requirements Documentation | Project has business logic AND no `docs/requirements/` | Already exists, OR project is a pure prompts/docs repo |
| 05 | README & Changelog | Always (covers README and CHANGELOG) | Both exist AND user did not ask to refresh |
| 06 | Javadoc | Project type is Library/SDK AND language is Java AND public API exists | Non-Java, no public API, or application-only project. For TS/Python/Rust/etc., note in summary that the prompt has language adaptation guidance but is Java-first |
| 07 | Developer Onboarding | Project has buildable source code | Pure docs/prompts repo with no build step |
| 08 | Contributing Guide | Project accepts contributions (has CI/CD or is in a shared repo) | Solo experimental repo with no CI |
| 09 | Test Strategy | Project has a test suite | No tests exist |
| 10 | CI/CD Pipeline | `.gitlab-ci.yml`, `.github/workflows/`, or `Jenkinsfile` exists | No CI/CD config |
| 11 | Release & Versioning SOP | Project type is Library/SDK OR project publishes versioned artifacts | Internal-only application with no version-based release process |
| 12 | Runbook | Project type is Service/API, Application, or Infrastructure (anything deployed and operated) | Library/SDK, CLI tool with no operations, pure docs repo |
| 13 | Troubleshooting Guide | Project has source code AND external dependencies OR runtime configuration | Pure docs/prompts repo, trivial scripts with no runtime concerns |
| 14 | Migration & Upgrade Guide | Project type is Library/SDK AND has 2+ major versions in history | Single major version, application that doesn't expose a consumer API |

#### Default Plan by Project Type

Use these as starting points, then adjust per the discovery findings:

**Library/SDK (Java):** 01, 02, 03, 05, 06, 07, 08, 09, 10, 11, 13, (14 if multi-major)

**Library/SDK (non-Java):** 01, 02, 03, 05, 07, 08, 09, 10, 11, 13, (14 if multi-major)

**Service/API:** 01, 02, 03, 05, 07, 08, 09, 10, 12, 13

**UI application:** 01, 02, 03, 05, 07, 08, 09, 10, 12, 13

**Application (CLI/batch):** 01, 02, 03, 05, 07, 08, 09, 10, 12, 13

**Infrastructure/IaC:** 01, 02, 05, 07, 08, 10, 12, 13

**Documentation/prompts repo:** 01, 05, 07, 08

**Monorepo:** Run discovery per package, then pick the union — usually 01, 02, 05, 07, 08, plus per-package additions

#### Order of Execution

Run prompts in this order (skipping any not in the plan). Earlier outputs feed later prompts:

1. **01 Bootstrap** — sets up `.kiro/` so later prompts can reference steering files
2. **02 Reverse Engineering** — produces `docs/architecture/*` that 03, 07, 08, 09, 11, 12, 13, 14 reference
3. **03 Requirements Documentation** — produces requirements that 13 references for failure modes
4. **05 README & Changelog** — produces README/CHANGELOG that 07, 08, 11, 14 reference
5. **06 Javadoc** — adds inline docs (independent — can run any time after 02)
6. **09 Test Strategy** — independent of later prompts but informs 08
7. **10 CI/CD Pipeline** — references 02 and 09 outputs
8. **08 Contributing Guide** — references 02, 05, 07, 09, 10
9. **07 Developer Onboarding** — references 02, 05, 09, 10 (run after CI/CD docs so onboarding can link to them)
10. **11 Release & Versioning SOP** — references 02, 05, 10
11. **12 Runbook** — references 02, 03, 10
12. **13 Troubleshooting Guide** — references 02, 03, 06 (integration map and config analysis), 10
13. **14 Migration Guide** — references 02, 05 (CHANGELOG), 11

### Phase 3: Confirm the Plan

Before running anything, present the plan to the user in this format:

```
**Project:** [name] ([type], [language/framework])

**Discovered capabilities:**
- Source code: [Yes/No]
- Tests: [framework, count]
- CI/CD: [platform or None]
- Deployment artifacts: [Docker/Helm/None]
- External integrations: [count, list 2-3 key ones]
- Existing docs: [list what already exists]
- Version history: [count of major versions]

**Plan — [N] prompts will run in this order:**
1. [01 Bootstrap] — sets up .kiro standards
2. [02 Reverse Engineering] — produces architecture docs
3. [05 README & Changelog] — generates README and CHANGELOG
   ...

**Skipped — [M] prompts:**
- 06 Javadoc — non-Java project
- 11 Release SOP — application, not a published library
- 14 Migration Guide — single major version
   ...

**Estimated time:** Each prompt takes [5-15 minutes]. Plan to confirm output between prompts.

Want me to:
(a) Run the full plan
(b) Run a subset — tell me which numbers
(c) Modify the plan first
```

Wait for confirmation. Do not start running prompts until the user approves.

### Phase 4: Execute the Plan

For each prompt in the approved plan:

1. **Announce the step** — "Starting prompt 02: Reverse Engineering. Phase 1 will discover and report; Phase 2 will write docs after your confirmation."
2. **Read the prompt file** from the prompts library location
3. **Execute the prompt's workflow** — every prompt in this set has its own Phase 1 (discovery) → Phase 2 (generation) → Phase 3 (validation) flow. Honor each prompt's "STOP HERE" checkpoints — the user must confirm Phase 1 findings before Phase 2 writes files.
4. **Verify the output** — after the prompt completes, run its Phase 3 validation checklist
5. **Update the running plan** — note which prompt finished, what files were created, and any flagged issues
6. **Move to the next prompt** — or stop if the user requests a break

If a prompt fails or the user wants to skip mid-execution, capture the state and offer to resume later.

### Phase 5: Final Summary

After the last prompt completes, present:

```
**Project setup complete.**

Prompts run: [N]
Files created: [count]
Files modified: [count]

**New artifacts:**
- .kiro/steering/[files]
- docs/architecture/01-*.md through 10-*.md
- docs/onboarding/developer-setup.md
- docs/testing/test-strategy.md
- docs/cicd/pipeline.md
- docs/operations/runbook.md
- docs/operations/release-process.md
- docs/troubleshooting.md
- docs/MIGRATION.md
- README.md (updated)
- CHANGELOG.md (updated)
- CONTRIBUTING.md

**Skipped (not applicable):**
- 06 Javadoc — [reason]
- 14 Migration Guide — [reason]

**Recommended next steps:**
1. Review generated docs and commit to a feature branch
2. Open an MR for team review
3. [Project-specific suggestions]
```

## Decision Heuristics

When discovery is ambiguous, use these rules:

- **No source code, only markdown** → Documentation/prompts repo. Run only 01, 05, 07, 08.
- **Has `Dockerfile` + `Helm` + endpoints** → Service. Include 12 (Runbook), exclude 11 (Release SOP) unless it publishes to a registry.
- **Has `pom.xml` with `<packaging>jar</packaging>` and a `groupId` from a known org** → Library. Include 06 (Javadoc) and 11 (Release SOP).
- **Has CI/CD that publishes a Docker image but is consumed as a service** → Application. Skip 11 unless versioning is consumer-facing.
- **Angular project with `ng build`** → UI application. Skip 06 (Javadoc — Angular projects use TSDoc; the prompt has language adaptation but is Java-first; mention this in the summary).
- **No tests at all** → Skip 09. Recommend in the summary that the team add tests before re-running.
- **No external integrations and no database** → Trim 13 (Troubleshooting) to focus on build/config/runtime issues only. Skip integration-related sections.
- **First commit is recent (single version)** → Skip 14 (Migration Guide). Note that it can be run later when the project ships its second major version.

## Rules

1. **Discover before deciding.** Never assume project type from the directory name. Read manifests, configs, and source.
2. **Preserve existing assets.** If `README.md` already exists, the README/Changelog prompt's own logic preserves manually written content. Honor that.
3. **Honor each prompt's confirmation checkpoint.** Every prompt has a "STOP HERE" between Phase 1 (discovery) and Phase 2 (generation). The user gets one approval per prompt, plus the up-front plan approval.
4. **Run sequentially, not in parallel.** Later prompts depend on outputs from earlier ones. Do not parallelize.
5. **Match the prompt's own rules.** Each prompt has its own validation checklist (no secrets, real commands, etc.). Run that checklist after each step.
6. **Tailor the skip list with reasons.** When skipping a prompt, always cite the discovery signal that triggered the skip.
7. **Don't fabricate signals.** If you can't determine whether the project has tests, ask the user — don't guess.
8. **Stop on failure.** If a prompt errors out or produces unusable output, pause and ask the user how to proceed before moving to the next.

## Dependencies

- Prompts library (see "Prompts Library Location" at the top) — required
- Access to your organization's standards repository (the "ai-rules" repo, used by prompt 01) — recommended
- `glab` CLI — optional, used by prompt 10 for pipeline log analysis
- Dynatrace MCP — optional, used by prompts 07, 12 for observability and runtime context

## Output

- A concrete execution plan tailored to the project type and discovered capabilities
- Files generated by each selected prompt at the prompt's documented output location
- A final summary listing what was created, what was skipped, and why
