---
title: "Issue Tracking System — Setup Workflow"
description: "Step-by-step workflow for discovering, configuring, and installing the markdown-based issue tracking system"
---

# Issue Tracking System — Setup Workflow

This steering file defines the complete setup flow. Follow each step in order.

## Step 1: Discover the Workspace

Before creating anything, scan the workspace to understand what already exists.

### 1a: Check for existing issue tracking

```
Search for:
- issue_tracking/ directory
- issues/ directory
- .github/ISSUE_TEMPLATE/ directory
- TODO.md, TODO, todo.txt
- Any markdown files with checkbox syntax (- [ ])
```

If an existing issue tracking system is found, report what's there and ask:

> I found an existing issue tracking setup in `[location]`. It has [N] files with [M] open items.
>
> Would you like to:
> 1. **Enhance it** — add missing files and hooks to the existing system
> 2. **Replace it** — start fresh (existing files will be backed up)
> 3. **Cancel** — keep what you have

### 1b: Discover related project files

Scan these locations and record what exists. Do NOT create any of these — only note their presence or absence.

**Changelogs:**
- `CHANGELOG.md`, `doc/CHANGELOG.md`, `docs/CHANGELOG.md`
- `CHANGES.md`, `HISTORY.md`, `NEWS.md`

**Design documents:**
- `doc/game-design.md`, `doc/design.md`, `docs/design.md`, `DESIGN.md`
- Any `*.md` files in `doc/` or `docs/` that contain design-related content

**Meeting notes / transcripts:**
- `doc/meeting_transcripts/`, `docs/meetings/`, `meetings/`, `notes/`
- Any directories containing transcript, meeting, or notes files

**Contributing guide:**
- `CONTRIBUTING.md`, `docs/CONTRIBUTING.md`, `.github/CONTRIBUTING.md`

**Architecture documentation:**
- `docs/architecture/`, `doc/architecture/`, `ARCHITECTURE.md`

**Roadmap:**
- `ROADMAP.md`, `docs/ROADMAP.md`, `doc/roadmap.md`

**Person-specific feedback files:**
- Any files in `issue_tracking/` matching `*-suggestions.md`, `*-feedback.md`, `*-notes.md`
- These are NOT part of the core system but should be referenced so triage can pull from them

**Project metadata:**
- `package.json`, `pom.xml`, `Cargo.toml`, `pyproject.toml`, `go.mod` — for project name
- `README.md` — for project description

### 1c: Present discovery results

Show the user what was found:

```
Here's what I found in your workspace:

**Project:** [name from package.json/README or directory name]

**Existing issue tracking:** [None / Found at issue_tracking/ with N files]

**Related files discovered:**
✅ CHANGELOG.md — doc/CHANGELOG.md
✅ Architecture docs — docs/architecture/ (10 files)
✅ Meeting transcripts — doc/meeting_transcripts/ (2 files)
✅ Contributing guide — CONTRIBUTING.md
❌ Roadmap — not found
❌ Design doc — not found

**Person-specific files:**
📋 nathan-suggestions.md — 5 open items (will be referenced for triage)

These related files will be referenced in the tracking README so triage
can cross-reference them. I won't create any files that don't exist.
```

## Step 2: Confirm the File Structure

Present the default category structure:

```
Here's the default issue tracking structure. Each category gets its own
markdown file with a tag shortcode for triage.

| # | File | Tag | Purpose |
|---|------|-----|---------|
| 1 | core-priorities.md | [core] | Must-do / ship-blocking items |
| 2 | known-bugs.md | [bug] | Confirmed bugs |
| 3 | nice-to-haves.md | [nth] | Polish and deferred items |
| 4 | backlog.md | [bl] | Future features and long-term ideas |
| 5 | completed.md | [cpt] | Done and verified |
| 6 | rejected.md | [rej] | Considered but decided against |

Plus:
- 00_README.md — system documentation and rules

Would you like to add, remove, or rename any categories?
```

If the user wants changes:
- Add new categories with a tag shortcode
- Remove categories they don't need
- Rename categories (update the tag shortcode too)

Record the final structure for file creation.

## Step 3: Create the Tracking Files

For each file in the confirmed structure, check if it already exists. Only create files that are missing.

### Creation order:
1. `issue_tracking/00_README.md` — the system documentation (see file-templates.md)
2. Each category file in order
3. Report what was created vs. what already existed

### README customization:
The README template includes:
- File table (from confirmed structure)
- Adding items instructions
- Assignment syntax
- Tag table (from confirmed structure, including any custom tags)
- Triage instructions
- Rules
- **Related Project Files** section — populated from Step 1b discovery results
- **Person-Specific Files** section — lists any discovered feedback/suggestion files

### Category file template:
Each category file gets a simple header:

```markdown
# [Category Name] ([tag])

[One-line description of what goes here.]

```

Leave the file mostly empty — the user will populate it.

## Step 4: Install Hooks

Create two Kiro hooks in `.kiro/hooks/`:

### 4a: Issue Sync Hook

File: `.kiro/hooks/issue-sync.kiro.hook`

```json
{
  "enabled": true,
  "name": "Issue Sync",
  "description": "Scan all issue tracking files and move tasks based on tags. Items marked [x] go to completed. Tagged items move to their target file. Duplicates are removed.",
  "version": "1.0.0",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "[DYNAMIC — built from confirmed structure, see below]"
  }
}
```

The sync prompt MUST include:
- List of ALL category files to read (from confirmed structure)
- Tag-to-file mapping for every category (including custom ones)
- Instructions to remove tags after moving
- Instructions to deduplicate across files
- Instructions to report what was moved

### 4b: Issue Triage Hook

File: `.kiro/hooks/issue-triage.kiro.hook`

```json
{
  "enabled": true,
  "name": "Issue Triage",
  "description": "Code-aware triage: scan issue files, check code for implemented items, flag mutually exclusive items, then call Issue Sync.",
  "version": "1.0.0",
  "when": {
    "type": "userTriggered"
  },
  "then": {
    "type": "askAgent",
    "prompt": "[DYNAMIC — built from confirmed structure, see below]"
  }
}
```

The triage prompt MUST include:
- List of ALL category files to read
- Instructions to check code for implemented items and mark them [x]
- Instructions to flag mutually exclusive items and prompt user
- Instructions to also read any person-specific files discovered in Step 1b
- Instructions to call Issue Sync after marking

### Hook prompt generation rules:
- List every file path explicitly (don't use wildcards)
- Include the full tag-to-file mapping
- Reference person-specific files by name if they were discovered
- Keep prompts self-contained — no external skill file dependencies

## Step 5: Prompt About Gaps

After everything is created, prompt the user about discovered gaps and potential additions:

```
Everything is set up. A few things I noticed:

**Gaps you might want to address:**
- ❌ No CHANGELOG.md found — consider adding one to track releases
- ❌ No roadmap found — a ROADMAP.md could help prioritize backlog items

**Files I referenced but didn't create:**
- 📋 doc/CHANGELOG.md — referenced in README for cross-referencing
- 📋 doc/meeting_transcripts/ — triage will scan these for action items

**Suggestions:**
- Consider adding a `[person]-suggestions.md` file for each team member
  who provides regular feedback (triage will automatically pull from these)
- The sync hook can be triggered by saying "sync" in chat
- The triage hook can be triggered by saying "triage" in chat

Want me to make any adjustments?
```

## Step 6: Summary

```
Issue tracking system installed.

**Files created:** [N] new files in issue_tracking/
**Files preserved:** [M] existing files left unchanged
**Hooks installed:** 2 (Issue Sync, Issue Triage)
**Related files wired in:** [K] project files referenced in README

**Quick commands:**
- Say "sync" to move tagged items between files
- Say "triage" to scan code for implemented items
- Add items with `- [ ] description` in any tracking file
- Tag items with [core], [bug], [nth], [bl], [cpt], [rej] to move them

The system is ready to use.
```
