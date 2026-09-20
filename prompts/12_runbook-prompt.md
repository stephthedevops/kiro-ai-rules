# Runbook / Operational Procedures Generator Prompt

> **Purpose**: Copy and paste this prompt into Kiro (or any AI coding assistant) to generate operational runbooks and standard operating procedures (SOPs) for any software project. The AI will analyze the codebase, infrastructure, external dependencies, and deployment configuration to produce runbooks that support production operations, incident response, and routine maintenance.
>
> **Standards**: Follows [PagerDuty Incident Response](https://response.pagerduty.com/) and [Google SRE Workbook](https://sre.google/workbook/table-of-contents/) runbook best practices, plus your organization's internal documentation standards.

---

## The Prompt

```
I need you to generate operational runbooks and standard operating procedures (SOPs) for this project. These documents should enable an operator or on-call engineer to perform routine maintenance, respond to incidents, and execute operational tasks without deep knowledge of the codebase. Follow the process below systematically.

## Phase 1: Operational Analysis (Do This First — Report Before Writing)

Analyze the project and report findings. I will confirm before you begin writing.

### 1.1 External Dependencies and SLAs

For each external system the project depends on:
- **Service name and type**: REST API, database, message queue, CLI tool, etc.
- **Connection details**: How the project connects (URL patterns, ports, protocols)
- **Authentication**: How credentials are managed (Vault, env vars, keystores)
- **Credential rotation**: How and when credentials expire and need renewal
- **Health check**: How to verify the service is healthy
- **Failure impact**: What happens to the application when this service is down
- **Fallback behavior**: Does the application degrade gracefully or fail hard?

### 1.2 Configuration and Secrets Management

Identify:
- **Configuration files**: Location, format, how they're deployed
- **Environment profiles**: How the application selects its environment config
- **Secrets**: What secrets exist, where they're stored, how they're rotated
- **Lease/TTL management**: Any time-limited credentials (Vault leases, OAuth tokens)
- **Configuration change process**: How to update config in each environment

### 1.3 Deployment and Release

Determine:
- **Deployment targets**: Environments (dev, int, QA, beta, prod), servers, containers
- **Deployment method**: CI/CD pipeline, manual deployment, RFC process
- **Artifact type**: JAR, WAR, Docker image, npm package
- **Rollback procedure**: How to revert a bad deployment
- **Smoke tests**: How to verify a deployment succeeded

### 1.4 Monitoring and Alerting

Check for:
- **Health endpoints**: Application health checks, readiness probes
- **Logging**: Where logs are written, log levels, structured logging
- **Metrics**: What metrics are collected, where they're stored
- **Alerts**: What conditions trigger alerts, who gets notified
- **Dashboards**: Monitoring dashboards (Dynatrace, Grafana, CloudWatch, etc.)

### 1.5 Data Management

Identify:
- **Databases**: Type, location, connection method
- **Backup/restore**: How data is backed up and how to restore
- **Data retention**: Policies for log rotation, audit trail retention
- **Migration**: Database migration tools and procedures

### 1.6 Routine Maintenance Tasks

Look for:
- **Dependency updates**: How and when dependencies are updated
- **Certificate renewal**: TLS certificates, keystores
- **Log rotation**: How logs are managed
- **Cache management**: What caches exist and how to clear them
- **Scheduled jobs**: Cron jobs, scheduled tasks, batch processes

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Generate Runbooks

After confirmation, generate runbooks with the following structure. Create separate sections for each operational domain. Skip sections that don't apply.

### Document Structure

```markdown
# Operational Runbook — [Project Name]

## Overview

This runbook provides standard operating procedures for maintaining and operating [Project Name] in all environments. It is intended for operators, on-call engineers, and developers performing operational tasks.

**Prerequisites**: Familiarity with the [Architecture Documentation](docs/architecture/) and access to the environments described in the [Configuration Analysis](docs/architecture/07-configuration-analysis.md).

---

## Table of Contents

- [Environment Reference](#environment-reference)
- [Health Checks](#health-checks)
- [Credential and Secrets Management](#credential-and-secrets-management)
- [Deployment Procedures](#deployment-procedures)
- [Incident Response](#incident-response)
- [Routine Maintenance](#routine-maintenance)
- [Rollback Procedures](#rollback-procedures)
- [Escalation](#escalation)

---

## Environment Reference

| Environment | Purpose | Config Profile | URL/Host | Notes |
|-------------|---------|---------------|----------|-------|
| Local | Development | `local` | localhost | [Notes] |
| Dev | Integration testing | `dev` | [URL] | [Notes] |
| QA | Quality assurance | `qa` | [URL] | [Notes] |
| Beta | Pre-production | `beta` | [URL] | [Notes] |
| Prod | Production | `prod` | [URL] | [Notes] |

---

## Health Checks

### Application Health

**How to verify the application is running correctly:**

```bash
# [Health check command or URL]
[command]
```

**Expected response**: [What a healthy response looks like]

**Unhealthy indicators**: [What to look for when something is wrong]

### External Service Health

| Service | Health Check | Expected Response | Failure Impact |
|---------|-------------|-------------------|----------------|
| [Service 1] | [How to check] | [Expected] | [What breaks] |
| [Service 2] | [How to check] | [Expected] | [What breaks] |

---

## Credential and Secrets Management

### [Secret/Credential Name]

**Storage**: [Where it's stored — Vault path, env var, keystore]
**Used by**: [Which component uses this credential]
**Rotation frequency**: [How often it needs to be rotated]
**Expiration behavior**: [What happens when it expires — error message, degraded service, outage]

**Rotation procedure**:
1. [Step-by-step rotation process]
2. [Verification step]

**Emergency rotation** (if compromised):
1. [Immediate steps]
2. [Verification]
3. [Notification requirements]

---

[Repeat for each credential/secret]

---

## Deployment Procedures

### Standard Deployment

**Pre-deployment checklist**:
- [ ] All tests pass on the target branch
- [ ] CI/CD pipeline is green
- [ ] CHANGELOG updated with new version entry
- [ ] RFC approved (if required for this environment)
- [ ] Rollback plan reviewed

**Deployment steps**:
1. [Step-by-step deployment process]
2. [Post-deployment verification]

### Hotfix Deployment

**When to use**: Critical production bug that can't wait for the next release cycle.

1. [Hotfix branch creation]
2. [Fix, test, review process]
3. [Expedited deployment steps]
4. [Post-deployment verification]
5. [Backport to main branch]

---

## Incident Response

### Severity Levels

| Severity | Definition | Response Time | Examples |
|----------|-----------|---------------|---------|
| P1 — Critical | Service down, data loss risk | Immediate | [Examples] |
| P2 — High | Major feature broken, workaround exists | < 4 hours | [Examples] |
| P3 — Medium | Minor feature broken, low impact | < 1 business day | [Examples] |
| P4 — Low | Cosmetic, minor inconvenience | Next sprint | [Examples] |

### Incident Response Procedure

1. **Assess**: Determine severity and impact
2. **Communicate**: Notify stakeholders per severity level
3. **Diagnose**: Use the [Troubleshooting Guide](docs/troubleshooting.md) to identify root cause
4. **Mitigate**: Apply immediate fix or workaround
5. **Resolve**: Deploy permanent fix
6. **Post-mortem**: Document what happened, why, and how to prevent recurrence

### Common Incident Scenarios

#### Scenario: [External Service] is Down

**Symptoms**: [What users/operators see]
**Impact**: [What functionality is affected]
**Immediate actions**:
1. [First response steps]
2. [Communication steps]
**Resolution**: [How to resolve once service is restored]

---

[Repeat for each common scenario]

---

## Routine Maintenance

### [Maintenance Task Name]

**Frequency**: [Daily / Weekly / Monthly / Quarterly / As needed]
**Owner**: [Team or role responsible]
**Estimated duration**: [Time]

**Procedure**:
1. [Step-by-step instructions]
2. [Verification step]

**Rollback** (if something goes wrong):
1. [How to undo this maintenance task]

---

[Repeat for each maintenance task]

---

## Rollback Procedures

### Application Rollback

**When to rollback**: [Criteria for deciding to rollback vs. fix forward]

**Procedure**:
1. [Step-by-step rollback process]
2. [Verification that rollback succeeded]
3. [Communication to stakeholders]

### Configuration Rollback

**Procedure**:
1. [How to revert configuration changes]
2. [Verification]

### Database Rollback

**Procedure**:
1. [How to revert database changes — if applicable]
2. [Verification]

---

## Escalation

| Level | Contact | When to Escalate |
|-------|---------|-----------------|
| L1 — Team | [Team name / channel] | First response, all issues |
| L2 — Lead | [Tech lead / manager] | P1/P2 unresolved after [time] |
| L3 — Management | [Director / VP] | P1 unresolved after [time], data breach |

---

*Last Updated: [DATE]*
```

## Phase 3: Validation

After generating the runbook, verify:

### Runbook Validation
- [ ] Every procedure has numbered, actionable steps
- [ ] Every procedure includes a verification step ("how to confirm it worked")
- [ ] Health check commands are real and would work if executed
- [ ] Credential rotation procedures are complete (not just "rotate the credential")
- [ ] Rollback procedures exist for every deployment type
- [ ] Incident severity levels are defined with response times
- [ ] Escalation contacts and criteria are specified
- [ ] Environment reference table matches actual environments from config
- [ ] No secrets, passwords, or tokens appear anywhere (use placeholders or Vault paths)
- [ ] Links to existing documentation (architecture docs, troubleshooting guide) are correct

## Rules

1. **Read the configuration** — derive environments, services, and credentials from actual config files
2. **Read the integration map** — understand all external dependencies and their failure modes
3. **Read the security architecture** — understand authentication flows and credential management
4. **Don't invent infrastructure** — only document what actually exists in the codebase and config
5. **Be specific about commands** — "Run `curl https://service/health`" not "Check the health endpoint"
6. **Include verification steps** — every procedure must end with "how to confirm it worked"
7. **Include rollback steps** — every change procedure must have a way to undo it
8. **No secrets** — use Vault paths, env var names, or placeholders — never actual values
9. **Pause for confirmation** — after Phase 1 analysis, stop and wait before writing

## Reference Files

When generating runbooks, consult these project files if they exist:
- `docs/architecture/06-integration-map.md` — External systems, URLs, authentication methods
- `docs/architecture/07-configuration-analysis.md` — Environment profiles, config properties
- `docs/architecture/05-security-architecture.md` — Auth flows, credential management, Vault
- `docs/architecture/04-data-flow-maps.md` — Data flows showing where failures propagate
- `docs/architecture/09-business-workflows.md` — Business workflows with operational implications
- `conf/` — Configuration files with environment-specific settings
- `.gitlab-ci.yml` / `.github/workflows/` — CI/CD pipeline for deployment procedures
- `README.md` — Project overview, team contact, migration notes
- `CHANGELOG.md` — Version history for rollback reference
- `docs/troubleshooting.md` — Troubleshooting guide (if already generated)

## Organization Standards Reference

Standards and templates are maintained in your organization's standards repository (set its location for your team). Referenced files:
- `steering/security-standards.md` — OWASP/NIST security and incident response requirements
- `steering/java-standards.md` — Java operational patterns
- `skills/glab-cli-operations/SKILL.md` — GitLab CLI for pipeline and deployment operations
```

---

## Usage Notes

### How to Use This Prompt

1. Open any project in your AI coding assistant
2. Copy the entire prompt above (everything between the outer triple backticks)
3. Paste it into the chat
4. The AI will analyze operational aspects, report findings, and wait for confirmation
5. After confirmation, it generates the runbook

### Output Location

| Output | Suggested Location |
|---|---|
| Operational Runbook | `docs/operations/runbook.md` or `docs/RUNBOOK.md` |

### Relationship to Other Prompts

- **Troubleshooting prompt** (`docs/prompts/troubleshooting-guide-prompt.md`) — Troubleshooting covers diagnosis; runbooks cover procedures
- **Developer Onboarding prompt** (`docs/prompts/developer-onboarding-guide-prompt.md`) — Onboarding covers dev setup; runbooks cover operational tasks
- **This prompt** — Generates operational procedures for production support

---

**Last Updated**: 2026-04-23 (CST)
