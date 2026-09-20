# Javadoc / Code Documentation Generator Prompt

> **Purpose**: Copy and paste this prompt into Kiro (or any AI coding assistant) to generate comprehensive Javadoc (or equivalent language-specific documentation comments) for a project's public API surface. The AI will analyze the codebase, identify public classes and methods, and produce inline documentation that helps consumers understand the API without reading the implementation.
>
> **Standards**: Follows [Oracle's Javadoc guidelines](https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html), [Google Java Style Guide Section 7](https://google.github.io/styleguide/javaguide.html#s7-javadoc), and your organization's internal documentation standards.

---

## The Prompt

```
I need you to generate comprehensive Javadoc (or equivalent code documentation comments) for this project's public API. The goal is to document every public class, method, and constant so that consumers of this code can understand the API without reading the implementation. Follow the process below systematically.

## Phase 1: API Surface Analysis (Do This First — Report Before Writing)

Analyze the project and report findings. I will confirm before you begin writing.

### 1.1 Public API Inventory

For each public class, catalog:
- **Class name and package**
- **Class purpose**: What it does (inferred from code, existing comments, tests, and architecture docs)
- **Public methods**: Name, parameters, return type, exceptions thrown
- **Public constants**: Name, type, value, purpose
- **Annotations**: Framework annotations that affect behavior (@ManagedBean, @ApplicationScoped, @WebFilter, etc.)
- **Design pattern**: Singleton, factory, builder, filter, etc.

### 1.2 Existing Documentation Check

For each class, check:
- **Existing Javadoc**: Is there any? Is it accurate? Is it complete?
- **Architecture docs**: Does `docs/architecture/` describe this class's behavior?
- **API catalog**: Does an endpoint/API catalog document the public methods?
- **Test documentation**: Do tests or approval notes explain behavior or edge cases?
- **Inline comments**: Are there useful comments that should be promoted to Javadoc?

### 1.3 Behavioral Analysis

For each public method, determine:
- **Purpose**: What does this method do?
- **Parameters**: What does each parameter represent? Are there constraints (not null, valid range, etc.)?
- **Return value**: What does the return value represent? Can it be null?
- **Side effects**: Does it modify state, write to a database, call an external service?
- **Exceptions**: What exceptions can be thrown and under what conditions?
- **Thread safety**: Is the method thread-safe? Are there synchronization concerns?
- **Boundary conditions**: What happens with null input, empty strings, edge cases?

### 1.4 Consumer Context

Identify:
- **Who consumes this API**: Other projects, frameworks, end users
- **How it's consumed**: As a dependency (JAR/npm package), via REST, via CLI
- **What consumers need to know**: Configuration requirements, initialization order, lifecycle

**STOP HERE. Report all findings and wait for my confirmation before proceeding to Phase 2.**

## Phase 2: Generate Javadoc

After confirmation, add Javadoc to each public class and method following these standards.

### Class-Level Javadoc

```java
/**
 * [One-sentence summary of what this class does — starts with a verb phrase.]
 *
 * <p>[Detailed description: how it works, when to use it, important constraints.
 * Multiple paragraphs are fine for complex classes. Use {@code} for code references
 * and {@link} for cross-references to other classes.]
 *
 * <p><strong>Thread Safety:</strong> [Thread-safe / Not thread-safe / Immutable]
 *
 * <p><strong>Usage Example:</strong>
 * <pre>{@code
 * // Show the most common usage pattern
 * MyClass instance = MyClass.getInstance();
 * Result result = instance.doSomething(param);
 * }</pre>
 *
 * @author [Original author if known]
 * @since [Version when this class was introduced]
 * @see [Related classes]
 */
```

### Method-Level Javadoc

```java
/**
 * [One-sentence summary of what this method does — starts with a verb phrase.]
 *
 * <p>[Additional details if needed: algorithm description, important constraints,
 * when to use this method vs alternatives.]
 *
 * @param paramName [description — starts with lowercase, no period at end]
 * @param otherParam [description]
 * @return [description of return value — what it represents, whether it can be null]
 * @throws ExceptionType [when this exception is thrown]
 * @throws OtherException [when this exception is thrown]
 * @since [version]
 * @see [related method or class]
 */
```

### Constant-Level Javadoc

```java
/**
 * [What this constant represents and where it's used.]
 *
 * <p>Value: {@value}
 */
public static final int MAX_UPLOAD_SIZE = 52428800;
```

### Javadoc Standards

1. **First sentence is the summary** — it appears in method/class listings. Make it count.
2. **Use verb phrases** — "Validates the action entity" not "This method validates..."
3. **Document parameters completely** — what they represent, valid values, null behavior
4. **Document return values** — what the return value means, null possibility
5. **Document exceptions** — when each exception is thrown, not just that it can be thrown
6. **Document thread safety** — especially for singletons and shared state
7. **Document boundary conditions** — null handling, empty collections, edge cases
8. **Use {@code}** for inline code references
9. **Use {@link}** for cross-references to other classes and methods
10. **Use <pre>{@code ...}</pre>** for multi-line code examples
11. **Don't state the obvious** — `@param name the name` adds no value
12. **Don't duplicate the method signature** — the reader can see the types already

### What NOT to Document

- **Private methods** — unless they contain complex algorithms worth explaining
- **Getters/setters** — unless they have side effects or validation logic
- **Override methods** — unless the override changes the contract (use {@inheritDoc} otherwise)
- **Self-explanatory code** — if the method name and signature tell the whole story, a brief summary is sufficient

### Language Adaptation

For non-Java projects, adapt the format:
- **TypeScript/JavaScript**: Use JSDoc (`/** ... */`) with `@param`, `@returns`, `@throws`
- **Python**: Use docstrings (Google style or NumPy style) with Args, Returns, Raises sections
- **Go**: Use godoc conventions (comment starts with function name)
- **Rust**: Use `///` doc comments with `# Examples`, `# Panics`, `# Errors` sections
- **C#**: Use XML documentation comments (`/// <summary>`, `/// <param>`, etc.)

## Phase 3: Validation

After generating Javadoc, verify:

### Javadoc Validation
- [ ] Every public class has a class-level Javadoc comment
- [ ] Every public method has a method-level Javadoc comment
- [ ] Every public constant has a description
- [ ] All @param tags match actual parameter names
- [ ] All @return tags are present for non-void methods
- [ ] All @throws tags match actual thrown exceptions
- [ ] First sentence of each Javadoc is a meaningful summary
- [ ] No Javadoc simply restates the method signature
- [ ] Thread safety is documented for shared/singleton classes
- [ ] Null behavior is documented for parameters and return values
- [ ] Cross-references ({@link}) point to real classes/methods
- [ ] Code examples compile (mentally verify syntax)
- [ ] No secrets or sensitive information in examples

## Rules

1. **Read the implementation** — understand what the code actually does before documenting it
2. **Read the tests** — tests reveal edge cases, expected behavior, and boundary conditions
3. **Read existing architecture docs** — they provide context for class-level descriptions
4. **Don't guess behavior** — if you can't determine what a method does, say so and flag it
5. **Don't duplicate architecture docs** — Javadoc should complement, not repeat, external docs
6. **Preserve existing accurate Javadoc** — only update or replace if it's wrong or incomplete
7. **Document the contract, not the implementation** — consumers care about what, not how
8. **Pause for confirmation** — after Phase 1 analysis, stop and wait before writing

## Reference Files

When generating Javadoc, consult these project files if they exist:
- `docs/architecture/02-endpoint-catalog.md` — Public API surface documentation
- `docs/architecture/01-module-inventory.md` — Module purposes and relationships
- `docs/architecture/08-code-organization.md` — Code patterns and conventions
- `docs/architecture/05-security-architecture.md` — Security-relevant behavior
- `src/test/` — Test classes reveal expected behavior and edge cases
- `src/test/resources/approvals/notes/` — Testing notes with behavioral observations
- `.kiro/steering/java-standards.md` — Java coding standards including Javadoc expectations
- `.kiro/steering/reverse-engineering.md` — Architecture patterns derived from the codebase

## Organization Standards Reference

Standards and templates are maintained in your organization's standards repository (set its location for your team). Referenced files:
- `steering/java-standards.md` — Google Java Style Guide, Javadoc requirements
- `steering/security-standards.md` — Security documentation requirements
```

---

## Usage Notes

### How to Use This Prompt

1. Open any project in your AI coding assistant
2. Copy the entire prompt above (everything between the outer triple backticks)
3. Paste it into the chat
4. Optionally add: "Focus on these classes: [list]" to scope the work
5. The AI will analyze the API surface, report findings, and wait for confirmation
6. After confirmation, it generates Javadoc for each class

### Scoping Options

- **Full project**: Run as-is for complete API documentation
- **Single class**: Add "Focus on [ClassName] only" at the end
- **Single package**: Add "Focus on the [package] package only" at the end
- **New code only**: Add "Only document classes that have no existing Javadoc" at the end

### Output

Javadoc is added directly to the source files. No separate output file is created.

### Relationship to Other Prompts

- **README & Changelog prompt** (`docs/prompts/readme-changelog-generator-prompt.md`) — README references the API; Javadoc documents it in detail
- **Developer Onboarding prompt** (`docs/prompts/developer-onboarding-guide-prompt.md`) — Onboarding guide points developers to Javadoc for API understanding
- **This prompt** — Generates inline code documentation for the public API

---

**Last Updated**: 2026-04-23 (CST)
