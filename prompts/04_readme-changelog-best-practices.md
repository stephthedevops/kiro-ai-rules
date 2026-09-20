# README & Changelog Best Practices — Research & Recommendations

> Research compiled from [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), [Common Changelog](https://common-changelog.org/), [Make a README](https://www.makeareadme.com/), [Best-README-Template](https://github.com/othneildrew/Best-README-Template), and industry guides. Content was rephrased for compliance with licensing restrictions.

---

## Part 1: Best Practices for README.md

### What the Industry Recommends

Based on research from [makeareadme.com](https://www.makeareadme.com/), [Best-README-Template](https://github.com/othneildrew/Best-README-Template) (14.9k stars), and 2026 guides:

#### Essential Sections (in order)

1. **Title + Badges** — Project name with 4-7 status badges
2. **Description** — One paragraph explaining what, why, and for whom
3. **Table of Contents** — For READMEs longer than ~100 lines
4. **Getting Started / Installation** — Prerequisites and step-by-step setup
5. **Usage** — Code examples showing the most common use case
6. **API Reference / Architecture** — For libraries and services
7. **Configuration** — Environment variables, config files
8. **Testing** — How to run the test suite
9. **Deployment** — CI/CD and release process
10. **Contributing** — How to contribute (or link to CONTRIBUTING.md)
11. **License** — License type with link to LICENSE file
12. **Contact / Acknowledgments** — Team info, credits

#### Badge Best Practices (2026)

Per [shields.io](https://shields.io/) and community guides:
- Place badges on the first line after the title
- Use dynamic badges that pull real data (pipeline status, coverage, version)
- Limit to 4-7 badges — more creates visual clutter
- Link every badge to a relevant page
- Group badges logically: build status, quality, metadata

#### What Makes a README Stand Out

- **Usage examples with real code** — not just "see docs"
- **Screenshots or GIFs** for UI projects
- **Architecture diagram** (Mermaid) for complex projects
- **Troubleshooting section** for common issues
- **FAQ** for frequently asked questions

---

## Part 2: Best Practices for CHANGELOG.md

### Keep a Changelog Standard

The [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) specification is the most widely adopted standard. Key principles:

1. **Changelogs are for humans, not machines** — don't dump git log
2. **Every version gets an entry** — no gaps
3. **Group changes by type**: Added, Changed, Deprecated, Removed, Fixed, Security
4. **Latest version first** — reverse chronological
5. **Show release dates** — ISO 8601 format (YYYY-MM-DD)
6. **Link versions** — each version header should link to a diff comparison
7. **Keep an [Unreleased] section** — track upcoming changes at the top

### Common Changelog Additions

[Common Changelog](https://common-changelog.org/) adds stricter rules on top of Keep a Changelog:

- **Reference issues/PRs** in every entry where applicable
- **Credit contributors** by name
- **Explain breaking changes** with migration instructions
- **Don't include internal-only changes** (refactors, CI tweaks) unless they affect consumers

### Anti-Patterns to Avoid

Per [Keep a Changelog](https://keepachangelog.com/en/1.1.0/):

- **Commit log dumps** — raw git log is noise, not a changelog
- **Ignoring deprecations** — users need warning before things break
- **Inconsistent entries** — a partial changelog is worse than none
- **Ambiguous dates** — always use YYYY-MM-DD
- **Missing breaking change notices** — the most important thing to communicate

---

## Part 3: Suggested Improvements to an Example Library's Files

> The suggestions below use a hypothetical shared library (`example-common`) as a worked example. Adapt them to your own project.

### README.md — Suggested Changes

| # | Suggestion | Rationale | Priority |
|---|-----------|-----------|----------|
| 1 | **Add a Table of Contents** | README is 150+ lines. A TOC helps navigation. Industry standard for long READMEs. | High |
| 2 | **Add Prerequisites section** | Document Java 8, Maven, JBoss requirements for building. Currently missing — a new developer wouldn't know what to install. | High |
| 3 | **Add Build & Test commands** | Add `mvn clean package`, `mvn test`, `make add-precommit-hook`. Currently no build instructions at all. | High |
| 4 | **Add Usage / Dependency Coordinates section** | This is a library — consumers need to know the Maven coordinates (`groupId:artifactId:version`) to add it as a dependency. | High |
| 5 | **Add License badge and section** | LICENSE file exists but isn't referenced in README. Add a badge and a one-line section. | Medium |
| 6 | **Add a Mermaid architecture diagram** | A simple module diagram in the README (not just linked from docs/) gives instant visual context. | Medium |
| 7 | **Add Troubleshooting / FAQ section** | Common issues like Vault connectivity, LDAP setup, virus scanner path — things a new developer would hit. | Medium |
| 8 | **Move Vault Migration to CHANGELOG** | The migration notes are version-specific (4.2.0 → 5.0.0) and belong in the changelog, not the README. Keep a brief note in README linking to the changelog entry. | Low |
| 9 | **Add "Related Projects" section** | List consumer projects (e.g., `example-app`, `example-report`, `example-rs`) as consumers. Helps developers understand the ecosystem. | Low |
| 10 | **Add code coverage badge** | If SonarQube or Jacoco reports coverage, add a dynamic badge. Builds trust. | Low |

### CHANGELOG.md — Suggested Changes

| # | Suggestion | Rationale | Priority |
|---|-----------|-----------|----------|
| 1 | **Add [Unreleased] section at the top** | Keep a Changelog standard. Tracks changes since the last release. Currently missing. | High |
| 2 | **Add version comparison links at the bottom** | Keep a Changelog recommends linking each version to a diff URL. For GitLab: `[5.0.0]: https://gitlab.com/.../compare/4.2.0...5.0.0` | High |
| 3 | **Use standard Keep a Changelog categories consistently** | Some entries use "Changed" but others don't use "Added", "Fixed", "Security" where they should. The virus scan module addition in 1.0.8.V1 should be "Added", not just "Changed". | Medium |
| 4 | **Add a format declaration at the top** | Standard practice: "The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html)." | Medium |
| 5 | **Curate SVN-era entries more aggressively** | Many SVN-era entries are just "git-svn-id" references with no human-readable description. These should be summarized or grouped into meaningful entries. | Medium |
| 6 | **Add "Security" category for Fortify entries** | Multiple versions mention "fortify" commits. These are security scan remediations and should use the "Security" category per Keep a Changelog. | Medium |
| 7 | **Move the "AWS Environment Support" and "Build System Maintenance" sections into their respective version entries** | These are currently standalone sections at the bottom. They should be folded into the version they belong to (e.g., AWS support into the version that was current in 2021). | Low |
| 8 | **Add migration guides for breaking changes** | Version 5.0.0 mentions the breaking change but the migration steps are in README. They should also be in the changelog entry itself. | Low |

### Prompt — Suggested Improvements

| # | Suggestion | Rationale |
|---|-----------|-----------|
| 1 | **Add "Unreleased" section generation** | The prompt doesn't explicitly tell the AI to create an [Unreleased] section. This is a Keep a Changelog requirement. |
| 2 | **Add version comparison link generation** | The prompt should instruct the AI to generate `[X.Y.Z]: https://platform/compare/prev...X.Y.Z` links at the bottom of the changelog. |
| 3 | **Add SVN migration handling** | The prompt handles this implicitly but should have explicit guidance for repos migrated from SVN (how to handle git-svn-id commits, SVN revision references). |
| 4 | **Add screenshot/GIF guidance for UI projects** | The prompt focuses on code projects. For UI projects (Angular, React), it should suggest including screenshots or demo GIFs. |
| 5 | **Add monorepo handling** | The prompt doesn't address monorepos where multiple packages have independent versioning. |

---

## Part 4: Reference Links

### README Standards
- [Make a README](https://www.makeareadme.com/) — Minimal viable README guide
- [Best-README-Template](https://github.com/othneildrew/Best-README-Template) — 14.9k star template
- [Shields.io](https://shields.io/) — Badge generation service
- [Awesome README](https://github.com/matiassingers/awesome-readme) — Curated list of great READMEs

### Changelog Standards
- [Keep a Changelog v1.1.0](https://keepachangelog.com/en/1.1.0/) — The de facto standard
- [Common Changelog](https://common-changelog.org/) — Stricter subset of Keep a Changelog
- [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html) — Version numbering standard
- [Conventional Commits](https://www.conventionalcommits.org/) — Commit message standard that enables automated changelog generation

---

*Last Updated: 2026-04-23 (CST)*
