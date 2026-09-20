# Kiro Project Bootstrap Prompt

> **Purpose**: Copy and paste this prompt into Kiro chat when setting up a new project. It will configure steering files, hooks, skills, MCP servers, and all project scaffolding from your organization's approved standards — regardless of tech stack.

---

## The Prompt

```
I need you to bootstrap this project with the full Kiro development environment. This includes steering files, hooks, skills, and MCP server configuration pulled from our organization's approved standards repository.

## Source of Truth

Your organization's approved AI rules, steering files, and MCP configurations live in a central standards repository (the "ai-rules" repo). Set its URL for your team:
<your-org-standards-repo-url>

Clone or fetch from that repo to get the latest approved files. All steering files, hooks, skills, and MCP configs in that repo are the canonical source. Do not invent standards — pull them from the repo.

## What to Set Up

### 1. Project Detection (Do This First)

Before copying any files, analyze this project to determine:
- **Tech stack**: Look at file extensions, dependency files (package.json, pom.xml, build.gradle, CMakeLists.txt, Cargo.toml, requirements.txt, go.mod, angular.json, etc.), and project structure
- **Frontend framework**: Angular, React, Vue, etc. (check for angular.json, .angular/, @angular dependencies in package.json)
- **Build system**: Maven, Gradle, npm, yarn, pnpm, Make, CMake, Cargo, pip, Angular CLI, etc.
- **Test framework**: JUnit, Jest, Vitest, Karma, Jasmine, Protractor, Cypress, pytest, Google Test, Catch2, etc.
- **CI/CD**: GitLab CI, GitHub Actions, Jenkins, etc.
- **Infrastructure**: Docker, Kubernetes, Helm, Terraform, etc.
- **Databases**: MongoDB, PostgreSQL, MySQL, Redis, Snowflake, etc.
- **Data warehouse/analytics**: Snowflake (check for snowflake-connector dependencies, `*.sql` files with Snowflake dialect, dbt projects, JDBC/ODBC Snowflake connection strings in config)
- **Cloud provider**: AWS, Azure, GCP, etc.

Report what you found before proceeding. I'll confirm before you start copying files.

### 2. Steering Files (.kiro/steering/)

From the ai-rules repo, copy over:

**Always include (every project needs these):**
- `core-rules.md` — Time/date verification, fundamental rules
- `security-standards.md` — OWASP Top 10, NIST compliance
- `documentation-standards.md` — AI-first documentation principles
- `documentation-maintenance.md` — Pre-commit documentation checks
- `conventional-commits.md` — Commit message standards
- `mermaid-diagram-standards.md` — Diagram standards

**Include based on detected tech stack:**
- Java detected → `java-standards.md`, `maven-standards.md` or gradle equivalent, `spring-boot-standards.md`, `spring-profiles-standards.md`, `spring-security-oauth2-standards.md`
- Angular detected → `angular-standards.md`, any TypeScript/Node standards available. Patterns: `**/*.component.ts`, `**/*.module.ts`, `**/*.service.ts`, `**/*.directive.ts`, `**/*.pipe.ts`, `**/angular.json`
- Node.js/TypeScript detected → any TypeScript/Node standards available
- C/C++ detected → any C/C++ standards available
- Python detected → any Python standards available
- Docker detected → `docker-standards.md`
- GitLab CI detected → `gitlab-ci-standards.md`
- Helm/K8s detected → `helm-standards.md`, `kubernetes-eks-standards.md`, `kubernetes-helm-standards.md`
- MongoDB detected → `mongodb-standards.md`
- AWS detected → `aws-infrastructure.md`, `aws-standards.md`, `spring-cloud-aws-standards.md`
- Swagger/OpenAPI detected → `openapi-swagger-standards.md`, `swagger-standards.md`
- OAuth2/JWT detected → `oauth2-jwt-standards.md`
- Snowflake detected → `snowflake-standards.md`. Patterns: `**/*.sql`, `**/snowflake*.properties`, `**/dbt_project.yml`. Covers Snowflake SQL dialect, warehouse/schema naming conventions, role-based access, query optimization, secure views, data sharing, connection pooling, and credential management via Vault

**Include the steering README.md** that inventories all active steering files for this project.

For each steering file, ensure the frontmatter follows this pattern:
```yaml
---
title: "Rule Title"
description: "What this rule covers"
version: "1.0.0"
lastUpdated: "YYYY-MM-DD"
lastUpdatedBy: "AI Assistant"
inclusion: "always" | "fileMatch"
patterns: ["glob/patterns/**"]
---
```

Use `inclusion: "always"` only for universal rules. Use `inclusion: "fileMatch"` with appropriate `patterns` for tech-specific rules.

### 3. Hooks (.kiro/hooks/)

From the ai-rules repo, copy over any approved hooks. At minimum, set up:

**Universal hooks (every project):**
- **GitLab MR Security Review** (`gitlab-mr-review.json`) — userTriggered hook that performs comprehensive security-focused code review on merge requests
- **Commit workflow hooks** — Pre-commit validation, conventional commit enforcement, documentation sync checks

**Tech-stack-specific hooks:**
- If the project has a test framework → post-file-edit hook to run relevant tests
- If the project has a linter → post-file-edit hook to run linting on save
- If the project has a build step → hook to validate builds
- If Angular detected → post-file-edit hook for `ng lint` on `*.ts` files, hook for `ng test --watch=false` on component/service changes
- If Snowflake detected → preToolUse hook to review SQL write operations for Snowflake best practices (warehouse sizing, query cost, role usage)

All hooks must follow this JSON schema:
```json
{
  "name": "Hook Name",
  "version": "1.0.0",
  "description": "What this hook does",
  "when": {
    "type": "fileEdited|fileCreated|fileDeleted|userTriggered|promptSubmit|agentStop|preToolUse|postToolUse|preTaskExecution|postTaskExecution",
    "patterns": ["*.ext"],
    "toolTypes": ["read|write|shell|web|spec|*"]
  },
  "then": {
    "type": "askAgent|runCommand",
    "prompt": "For askAgent",
    "command": "For runCommand"
  }
}
```

### 4. MCP Server Configuration (.kiro/settings/mcp.json)

From the ai-rules repo, pull the approved MCP server configurations. Set up:

**Standard MCP servers (if available in the repo):**
- Any documentation servers
- Any code analysis servers
- Any security scanning servers

Merge with any existing `.kiro/settings/mcp.json` — do NOT overwrite existing configs.

### 5. Skills (.kiro/skills/)

From the ai-rules repo, copy over any approved skills. At minimum ensure:
- **prime** skill exists — for scanning and understanding project structure

### 6. Docs Directory

Ensure a `docs/` directory exists with at minimum a `README.md` that describes the documentation structure.

### 7. Scripts

From the ai-rules repo, copy over any utility scripts (like `update-rule-versions.sh` for steering file versioning).

## Rules for This Setup

1. **Do not invent standards** — only use what exists in the ai-rules repo
2. **Do not overwrite existing files** — merge or ask me before replacing anything
3. **Respect .gitignore** — don't add files that should be ignored
4. **Report what you did** — after setup, give me a summary of:
   - Detected tech stack
   - Files copied/created
   - Files skipped (already existed)
   - Any manual steps I need to take
5. **Validate the setup** — after copying, check for any obvious issues (missing dependencies, broken references, etc.)

## After Setup

Once everything is in place:
1. Show me the full `.kiro/` directory structure
2. List all steering files with their inclusion type
3. List all hooks with their trigger type
4. List any MCP servers configured
5. Flag anything that needs manual attention
```

---

## Usage Notes

### How to Use This Prompt

1. Open a new project in Kiro
2. Open the Kiro chat
3. Copy the entire prompt above (everything between the triple backticks)
4. Paste it into the chat
5. Kiro will analyze your project, pull from the ai-rules repo, and set everything up

### What Gets Detected Automatically

| Tech Stack | Detection Signal | Steering Files Applied |
|---|---|---|
| Java/Spring | `pom.xml`, `build.gradle`, `*.java` | java-standards, spring-boot, maven, spring-profiles, spring-security |
| Angular | `angular.json`, `@angular/*` in package.json, `*.component.ts` | angular-standards, TypeScript/Node standards |
| Node.js | `package.json`, `*.ts`, `*.js` | Any available Node/TS standards |
| C/C++ | `CMakeLists.txt`, `Makefile`, `*.cpp`, `*.h` | Any available C/C++ standards |
| Python | `requirements.txt`, `pyproject.toml`, `*.py` | Any available Python standards |
| Snowflake | `snowflake-connector` deps, `*.sql` with Snowflake dialect, `dbt_project.yml`, Snowflake JDBC/ODBC in config | snowflake-standards |
| Docker | `Dockerfile`, `docker-compose.yml` | docker-standards |
| Kubernetes | `deploy/*.yml`, `helm/` | helm-standards, kubernetes-standards |
| MongoDB | `*Repository.java`, `*Mongo*.java` | mongodb-standards |
| AWS | `application*.properties` with AWS refs | aws-infrastructure, aws-standards |
| GitLab CI | `.gitlab-ci.yml` | gitlab-ci-standards |

### Customizing the Prompt

- **Add tech stacks**: If your org adds new standards (e.g., Rust, Go), add detection rules and steering file mappings to the prompt
- **Add hooks**: Define new hook patterns in the "Tech-stack-specific hooks" section
- **Add MCP servers**: As new MCP servers are approved, add them to section 4
- **Remove sections**: If a project definitely won't need certain standards, you can trim the prompt

### Keeping It Current

This prompt references your organization's standards repository (the "ai-rules" repo) as the source of truth. As that repo is updated with new standards, this prompt automatically picks up the changes — no prompt updates needed for content changes. Only update this prompt if:
- New tech stack detection patterns are needed
- The `.kiro/` directory structure changes
- New Kiro features (beyond steering, hooks, skills, MCP) are added

---

**Last Updated**: 2026-04-23 (CST)
