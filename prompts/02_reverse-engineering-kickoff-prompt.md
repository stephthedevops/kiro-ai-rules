# Reverse Engineering Kickoff Prompt

> **Purpose**: Copy and paste this prompt into Kiro chat when you need to reverse-engineer and document an existing codebase. It produces comprehensive architecture documentation regardless of tech stack — Java/Spring, Angular, Node.js, C/C++, Python, Snowflake, or any combination.

---

## The Prompt

```
I need you to reverse-engineer this project and produce comprehensive architecture documentation. This should work regardless of tech stack. Follow the process below systematically.

## Source of Truth

Your organization's approved AI rules and documentation standards live in a central standards repository (the "ai-rules" repo). Set its URL for your team:
<your-org-standards-repo-url>

Pull documentation standards, security standards, and any reverse-engineering steering templates from that repo. Do not invent documentation patterns — use what the repo provides.

## Phase 1: Project Discovery (Do This First — Report Before Proceeding)

Analyze the project and report findings for each category. I will confirm before you begin documentation.

### 1.1 Technology Stack Detection

Scan the project root and source directories to identify:

**Language & Runtime:**
- Java → `pom.xml`, `build.gradle`, `*.java` (note Java version from compiler settings)
- TypeScript/Angular → `angular.json`, `@angular/*` in package.json, `*.component.ts`, `*.module.ts`, `*.service.ts`
- Node.js → `package.json`, `*.ts`, `*.js` (note if Express, NestJS, Fastify, etc.)
- C/C++ → `CMakeLists.txt`, `Makefile`, `*.cpp`, `*.c`, `*.h`, `*.hpp`
- Python → `requirements.txt`, `pyproject.toml`, `setup.py`, `*.py` (note if Django, Flask, FastAPI, etc.)
- Other → detect from file extensions and dependency manifests

**Framework & Libraries:**
- Spring Boot → `spring-boot-starter-*` dependencies, `@SpringBootApplication`
- Angular → `angular.json`, `@angular/core`, `@angular/cli`
- React → `react`, `react-dom` in package.json
- .NET → `*.csproj`, `*.sln`
- Read the dependency manifest completely — list all significant dependencies with versions

**Build System:**
- Maven (`pom.xml`) — extract parent, properties, plugins, profiles
- Gradle (`build.gradle`, `build.gradle.kts`) — extract plugins, dependencies, tasks
- npm/yarn/pnpm (`package.json`) — extract scripts, dependencies, devDependencies
- Angular CLI (`angular.json`) — extract build/serve/test configurations
- CMake (`CMakeLists.txt`) — extract targets, libraries, compile options
- Make (`Makefile`) — extract targets and build rules
- pip/poetry (`requirements.txt`, `pyproject.toml`) — extract dependencies

**Databases & Data Stores:**
- MongoDB → connection strings, Spring Data MongoDB, Mongoose
- PostgreSQL/MySQL → JDBC drivers, connection pools, ORM config
- Redis → cache configuration, connection settings
- Snowflake → `snowflake-connector-*` dependencies, JDBC/ODBC drivers, `snowflake.*` properties, dbt project files, warehouse/schema/role references in config or SQL files
- Elasticsearch → client dependencies, index configuration
- S3/Blob storage → cloud SDK usage for object storage

**Infrastructure & Deployment:**
- Docker → `Dockerfile`, `docker-compose.yml`
- Kubernetes → `deploy/*.yml`, Helm charts, `values.yaml`
- CI/CD → `.gitlab-ci.yml`, `.github/workflows/`, `Jenkinsfile`
- Cloud → AWS SDK, Azure SDK, GCP client libraries

**Authentication & Security:**
- OAuth2/OIDC → token validation, issuer config, JWT handling
- LDAP/AD → directory service integration
- SAML → SSO configuration
- API keys → key management patterns
- RBAC → role definitions, permission models

### 1.2 Project Structure Analysis

Map the directory tree (2-3 levels deep) and identify:
- Source code root(s)
- Test directories and test framework
- Configuration file locations
- Documentation directories
- Build output directories
- Infrastructure/deployment files

### 1.3 Entry Point Identification

Find the application entry point(s):
- Java → class with `main()` or `@SpringBootApplication`
- Angular → `main.ts`, `app.module.ts` or `app.config.ts` (standalone)
- Node.js → `main` field in package.json, or `index.ts`/`server.ts`/`app.ts`
- C/C++ → `main()` function location
- Python → `__main__.py`, `manage.py`, `app.py`, `main.py`

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Architecture Documentation

After confirmation, produce the following documentation artifacts. Create each as a separate file under `docs/architecture/`.

### 2.1 Module Inventory (`01-module-inventory.md`)

For each logical module/feature area in the codebase:

| Module | Package/Directory | Purpose | Key Classes/Files | Dependencies |
|---|---|---|---|---|
| [name] | [path] | [what it does] | [main files] | [other modules it depends on] |

**Tech-specific guidance:**
- **Java/Spring**: Map packages under the base package, identify `@RestController`, `@Service`, `@Repository`, `@Configuration` classes per module
- **Angular**: Map feature modules (`*.module.ts`), standalone components, shared modules, core module. Identify components, services, guards, interceptors, pipes, directives per module
- **Node.js**: Map route files, service files, middleware, models per feature directory
- **C/C++**: Map source directories, header organization, library boundaries
- **Python**: Map packages, modules, class hierarchies per feature area
- **Snowflake**: Map schemas, stored procedures, UDFs, views, stages, pipes per functional area

Include a Mermaid module dependency diagram.

### 2.2 Endpoint/Interface Catalog (`02-endpoint-catalog.md`)

**For REST APIs (Java/Spring, Node.js, Python):**

| Method | Path | Controller/Handler | Auth Required | Request Body | Response | Description |
|---|---|---|---|---|---|---|
| GET | /api/... | ClassName.method | Yes/No | DTO type | Response type | What it does |

**For Angular applications:**

| Route Path | Component | Guard(s) | Lazy Loaded | Module | Description |
|---|---|---|---|---|---|
| /path | ComponentName | AuthGuard | Yes/No | FeatureModule | What it shows |

Also document:
- HTTP interceptors and their order
- Route resolvers and their data
- State management patterns (NgRx, services, signals)

**For Snowflake:**

| Object Type | Schema | Name | Parameters | Returns | Description |
|---|---|---|---|---|---|
| Stored Proc | SCHEMA_NAME | proc_name | (params) | return type | What it does |
| View | SCHEMA_NAME | view_name | — | columns | What it exposes |
| UDF | SCHEMA_NAME | func_name | (params) | return type | What it computes |

**For C/C++:**

| Header | Function/Class | Visibility | Parameters | Returns | Description |
|---|---|---|---|---|---|
| file.h | function_name | public/internal | (params) | return type | What it does |

### 2.3 Schema/Data Model Catalog (`03-schema-catalog.md`)

**For databases (MongoDB, PostgreSQL, Snowflake, etc.):**

Document every entity/table/collection:

| Entity/Table | Collection/Table Name | Key Fields | Indexes | Relationships | Description |
|---|---|---|---|---|---|
| ClassName | db_name | field: type | index list | references | What it stores |

**For Angular:**
- Document TypeScript interfaces and models
- Document form models and validation schemas
- Document API request/response types

**For Snowflake specifically:**
- Document warehouse configurations (size, auto-suspend, auto-resume)
- Document database/schema hierarchy
- Document table structures with clustering keys
- Document external stages and file formats
- Document data sharing configurations
- Document role hierarchy and grants

Include a Mermaid entity relationship diagram.

### 2.4 Data Flow Maps (`04-data-flow-maps.md`)

Trace how data moves through the system:

**For full-stack applications (e.g., Angular + Java + Snowflake):**
```
User → Angular Component → Angular Service → HTTP Interceptor → API Gateway →
Spring Controller → Spring Service → Repository → MongoDB/Snowflake
```

**For each major workflow**, create a Mermaid sequence diagram showing:
- User/client action
- Frontend processing (if applicable)
- API call chain
- Service layer processing
- Database operations
- External system calls
- Response path

**For Snowflake data pipelines:**
- Source → Stage → COPY INTO → Raw table → Transform (stored proc/dbt) → Analytics table → View
- Document Snowpipe configurations
- Document task/stream patterns for CDC
- Document data sharing flows

### 2.5 Security Architecture (`05-security-architecture.md`)

Document the complete security model:

**Authentication:**
- How users authenticate (OAuth2, LDAP, SAML, API keys, Angular route guards)
- Token format and validation
- Session management (stateless JWT, cookie-based, etc.)

**Authorization:**
- Role definitions and hierarchy
- Permission model (RBAC, ABAC, annotation-based, explicit checks)
- For Angular: route guards, directive-based visibility, service-level checks
- For Snowflake: role hierarchy, database/schema grants, row-level security, masking policies

**Data Protection:**
- Encryption at rest and in transit
- Sensitive data handling
- PII/PHI identification
- For Snowflake: dynamic data masking, secure views, network policies

**Security Checklist** (per OWASP Top 10):
- [ ] Input validation on all entry points
- [ ] Parameterized queries / ORM usage
- [ ] XSS prevention (output encoding, Angular sanitization, CSP headers)
- [ ] CSRF protection
- [ ] Authentication on all protected endpoints/routes
- [ ] Authorization checks at every layer
- [ ] Secrets management (no hardcoded credentials)
- [ ] Dependency vulnerability scanning
- [ ] Security event logging
- [ ] Error handling without information disclosure

### 2.6 Integration Map (`06-integration-map.md`)

Document every external system the application communicates with:

| System | Protocol | Direction | Auth Method | Config Location | Purpose |
|---|---|---|---|---|---|
| [name] | REST/JDBC/AMQP/etc. | Inbound/Outbound/Both | OAuth2/API Key/etc. | [config file] | What it does |

**Tech-specific integrations:**
- **Java/Spring**: RestTemplate/WebClient calls, JMS/SQS listeners, SMTP, LDAP
- **Angular**: HTTP service calls, WebSocket connections, SSE, third-party SDK integrations (analytics, auth providers)
- **Snowflake**: External functions, external tables, data shares, Snowpipe, connectors (Kafka, Spark), partner integrations
- **Node.js**: HTTP clients, message queue consumers/producers, gRPC
- **C/C++**: Socket connections, shared libraries, IPC mechanisms

Include a Mermaid integration diagram showing all external touchpoints.

### 2.7 Configuration Analysis (`07-configuration-analysis.md`)

**For each environment** (local, dev, int/staging, beta, prod):

| Property/Setting | Dev | Staging | Prod | Description |
|---|---|---|---|---|
| database.url | ... | ... | ... | Database connection |
| auth.issuer | ... | ... | ... | Auth provider |

**Tech-specific configuration:**
- **Java/Spring**: `application*.properties`/`application*.yml`, Spring profiles, Vault integration, `@Value` and `@ConfigurationProperties` usage
- **Angular**: `environment.ts` files, `angular.json` build configurations, proxy configs, feature flags
- **Node.js**: `.env` files, config modules, environment variable usage
- **Snowflake**: Warehouse configurations per environment, role assignments, network policies, resource monitors
- **C/C++**: Preprocessor defines, build-time configuration, runtime config files

Document:
- Secrets management approach (Vault, AWS SSM, environment variables, Angular environment files)
- Feature flags and toggles
- Environment-specific behavior differences

### 2.8 Code Organization & Patterns (`08-code-organization.md`)

Document the architectural patterns and conventions used:

**Universal patterns to identify:**
- Layered architecture (controllers → services → repositories)
- Module/feature organization
- Dependency injection patterns
- Error/exception handling strategy
- Logging strategy and levels
- Naming conventions (classes, methods, files, variables)

**Java/Spring specific:**
- Interface + Impl pattern
- Converter/Mapper pattern
- `@ControllerAdvice` exception handling
- Spring event listeners
- Startup initialization (`CommandLineRunner`, `@PostConstruct`)
- Caching strategy (`@Cacheable`, `@CacheEvict`)

**Angular specific:**
- Smart/dumb component pattern (container vs presentational)
- Service injection patterns (providedIn: 'root' vs module-scoped)
- State management approach (NgRx, BehaviorSubject services, signals)
- Reactive patterns (Observable chains, async pipe usage)
- Form patterns (reactive forms vs template-driven)
- Lazy loading and route configuration
- Shared module organization
- Interceptor chain order
- Custom pipe and directive patterns
- Change detection strategy (OnPush vs Default)

**Snowflake specific:**
- Schema organization (raw, staging, analytics, reporting)
- Stored procedure patterns (JavaScript, SQL, Python)
- View layering (raw views → business views → secure views)
- Task and stream patterns for data pipelines
- Naming conventions (warehouses, databases, schemas, objects)
- Tagging and classification policies
- Cost management patterns (warehouse sizing, clustering, materialized views)

**Node.js specific:**
- Middleware chain pattern
- Route organization
- Error handling middleware
- Dependency injection (if using NestJS or similar)

**C/C++ specific:**
- Header/source organization
- Namespace usage
- Memory management patterns (RAII, smart pointers)
- Build target organization

### 2.9 Business Workflow Documentation (`09-business-workflows.md`)

For each major business process in the application:

**Document the lifecycle/state machine:**
- States and transitions
- Who can trigger each transition
- Side effects of each transition (emails, notifications, external calls)
- Validation rules at each step

Create a Mermaid state diagram for each workflow.

**For Angular applications**, also document:
- User journey through the UI (page flow)
- Form submission workflows
- Multi-step wizard patterns
- Optimistic vs pessimistic update patterns

**For Snowflake data pipelines**, also document:
- Data ingestion workflows (batch, streaming, CDC)
- Transformation pipeline stages
- Data quality check workflows
- Reporting/analytics refresh schedules

### 2.10 Technology Stack Summary (`10-tech-stack-summary.md`)

Create a comprehensive technology inventory:

| Category | Technology | Version | Purpose | Notes |
|---|---|---|---|---|
| Language | Java/TypeScript/etc. | X.Y | Primary language | ... |
| Framework | Spring Boot/Angular/etc. | X.Y.Z | Application framework | ... |
| Database | MongoDB/Snowflake/etc. | X.Y | Data storage | ... |
| Build | Maven/Angular CLI/etc. | X.Y | Build tool | ... |
| ... | ... | ... | ... | ... |

Include:
- Deprecated dependencies flagged with recommended replacements
- Known security vulnerabilities in current versions
- Upgrade path recommendations

## Phase 3: Steering File Generation

After documentation is complete, generate a reverse-engineering steering file at `.kiro/steering/reverse-engineering.md` that captures:

1. The discovered architecture patterns and conventions
2. Module structure and naming conventions
3. Authorization patterns
4. Integration patterns
5. Key conventions and gotchas (intentional misspellings, circular dependencies, etc.)

Use `inclusion: "fileMatch"` with patterns matching the project's source files.

Follow the frontmatter format:
```yaml
---
title: "Reverse Engineering - [Project Name]"
description: "Architecture patterns and conventions derived from reverse-engineering the [Project Name] codebase"
version: "1.0.0"
lastUpdated: "YYYY-MM-DD"
lastUpdatedBy: "AI Assistant"
inclusion: "fileMatch"
patterns: ["src/**/*.<ext>", "dependency-manifest"]
---
```

## Phase 4: Completeness Checklist

Before declaring the reverse engineering complete, verify:

- [ ] All source directories scanned
- [ ] All modules/features documented
- [ ] All endpoints/routes cataloged
- [ ] All entities/models documented
- [ ] All external integrations mapped
- [ ] All configuration properties documented
- [ ] All security mechanisms documented
- [ ] All business workflows documented with state diagrams
- [ ] All data flows traced with sequence diagrams
- [ ] Technology stack fully inventoried
- [ ] Mermaid diagrams use valid syntax and render correctly
- [ ] Steering file generated with discovered patterns
- [ ] Cross-references between documents are valid
- [ ] No hardcoded secrets exposed in documentation

## Rules

1. **Read the code** — do not guess. Open files and trace actual call chains.
2. **Be specific** — use actual class names, method names, file paths, and line references.
3. **Flag unknowns** — if something is unclear or ambiguous, flag it rather than assuming.
4. **Security first** — never include actual secrets, passwords, or tokens in documentation. Use placeholders.
5. **Follow existing standards** — pull documentation and diagram standards from the ai-rules repo.
6. **Pause for confirmation** — after Phase 1 discovery, stop and wait for my go-ahead before producing documentation.
```

---

## Usage Notes

### How to Use This Prompt

1. Open the project you want to reverse-engineer in Kiro
2. Open the Kiro chat
3. Copy the entire prompt above (everything between the outer triple backticks)
4. Paste it into the chat
5. Kiro will scan the project, report findings, and wait for your confirmation before documenting

### What Gets Produced

| Phase | Output | Location |
|---|---|---|
| Phase 1 | Tech stack report | Chat (for your review) |
| Phase 2 | 10 architecture docs | `docs/architecture/01-*.md` through `10-*.md` |
| Phase 3 | Steering file | `.kiro/steering/reverse-engineering.md` |
| Phase 4 | Completeness checklist | End of chat session |

### Tech Stack Coverage

| Tech Stack | Discovery Signals | Documentation Sections Tailored |
|---|---|---|
| Java/Spring | `pom.xml`, `*.java`, `@SpringBootApplication` | Modules, endpoints, converters, Spring patterns, profiles |
| Angular | `angular.json`, `*.component.ts`, `@angular/*` | Routes, components, services, guards, interceptors, state management, reactive patterns |
| Node.js | `package.json`, `*.ts`/`*.js` | Routes, middleware, services, error handling |
| C/C++ | `CMakeLists.txt`, `Makefile`, `*.cpp`/`*.h` | Headers, libraries, build targets, memory patterns |
| Python | `requirements.txt`, `*.py` | Modules, routes/views, ORM models, middleware |
| Snowflake | `snowflake-connector-*`, `*.sql`, `dbt_project.yml` | Schemas, procedures, views, warehouses, roles, pipelines, cost patterns |
| Docker | `Dockerfile`, `docker-compose.yml` | Build stages, security, health checks |
| Kubernetes | `deploy/*.yml`, Helm charts | Deployment config, resource limits, probes |
| AWS | AWS SDK deps, `application*.properties` | S3, SQS, SNS, SSM, IAM integration |
| GitLab CI | `.gitlab-ci.yml` | Pipeline stages, deployment environments |

### Adapting for Your Project

The prompt is designed to be copy-pasted as-is. Kiro will automatically:
- Skip sections that don't apply (no Angular docs for a pure Java project)
- Detect the right patterns for your stack
- Use appropriate terminology (controllers vs handlers vs views)

If you want to focus on specific areas, add a line at the end of the prompt:
```
Focus areas: [list specific areas, e.g., "security architecture and data flows only"]
```

### Relationship to Bootstrap Prompt

- **Bootstrap prompt** → sets up Kiro's development environment (steering, hooks, MCP)
- **Reverse engineering prompt** → documents an existing codebase's architecture

Use the bootstrap prompt first to set up standards, then the reverse engineering prompt to document what's already there. The reverse engineering output feeds back into the steering file, which then guides future development.

---

**Last Updated**: 2026-04-23 (CST)
