---
title: "Issue Tracking System — File Templates"
description: "Templates for all issue tracking files, README, and hook prompts"
---

# Issue Tracking System — File Templates

Templates used during setup. All templates are customized based on the user's confirmed structure and discovered project files.

## 00_README.md Template

```markdown
# Issue Tracking System

How we track work on [PROJECT_NAME].

## Files

| File | Purpose | What goes here |
|------|---------|---------------|
[DYNAMIC — one row per confirmed category]

## Adding an Item

Add a checkbox line to the appropriate file:

\`\`\`
- [ ] Short description of the task
\`\`\`

If you're not sure which file, add it to `backlog.md` and tag it for triage.

## Assigning Work

Add `@yourname` at the end of an item to claim it:

\`\`\`
- [ ] Balance pass on tile claim cost @steph
- [ ] Deploy to production @nathan
\`\`\`

Multiple people can be assigned:
\`\`\`
- [ ] Playtest with 4+ players @steph @nathan
\`\`\`

To unassign, just remove the `@name`.

## Tags

Tag items to move them during triage. Add the tag in brackets after the checkbox:

| Tag | Moves to |
|-----|----------|
[DYNAMIC — one row per confirmed category with tag shortcode]

Example:
\`\`\`
- [core] This should be a core priority
- [bl] Move this to backlog
\`\`\`

Items marked `[x]` are automatically moved to completed during triage.

## Triage (Kiro AI)

Say **"sync"** or **"triage"** in the Kiro chat to trigger automatic issue management:

**Sync** — moves tagged items to the correct file, deduplicates, moves `[x]` to completed
**Triage** — scans code for implemented items, flags conflicts, then runs sync

## Rules

- Each item lives in **one file only** — no duplicates
- Use tags to request moves, or just edit the files directly
- Mark items `[x]` when done — triage will move them to completed
- Keep descriptions short and clear
- Group items under section headers (## Gameplay, ## UI, etc.)

[IF_RELATED_FILES]
## Related Project Files

These files are cross-referenced during triage for context:

| File | Type |
|------|------|
[DYNAMIC — one row per discovered related file]

[END_IF_RELATED_FILES]

[IF_PERSON_FILES]
## Person-Specific Files

Feedback and suggestion files from team members. Triage scans these for items to pull into the main tracking files.

| File | Owner |
|------|-------|
[DYNAMIC — one row per discovered person-specific file]

[END_IF_PERSON_FILES]
```

## Category File Template

Used for each category file (core-priorities.md, known-bugs.md, etc.):

```markdown
# [CATEGORY_DISPLAY_NAME] ([TAG])

[CATEGORY_DESCRIPTION]

```

### Default category descriptions:

| Category | Display Name | Description |
|----------|-------------|-------------|
| core-priorities | Core Priorities | Must-do items. Ship blockers, deployment requirements, deadline-critical work. |
| known-bugs | Known Bugs | Confirmed bugs that need fixing. Anything broken, crashing, or behaving wrong. |
| nice-to-haves | Nice-to-Haves | Deferred features and polish items. Not required but would improve the project. |
| backlog | Backlog | Future features and long-term ideas. Not blocking current work but worth building later. |
| completed | Completed | Items that have been implemented and verified. |
| rejected | Rejected | Items that were considered but decided against. Include the reason. |

## Issue Sync Hook Prompt Template

```
Read ALL files in the issue_tracking/ directory ([COMMA_SEPARATED_FILE_LIST]).
For each task line, check for these tags/markers and move the task to the correct file:

[DYNAMIC — one line per category:]
- Items marked [x] or tagged '[TAG]' → move to issue_tracking/[FILENAME]

After moving, remove the tag from the task text.
Remove any duplicates (same task appearing in multiple files).
Then give a brief summary of what was moved.
```

## Issue Triage Hook Prompt Template

```
Read ALL files in the issue_tracking/ directory ([COMMA_SEPARATED_FILE_LIST]).
[IF_PERSON_FILES]Also read these person-specific files: [COMMA_SEPARATED_PERSON_FILES].[END_IF_PERSON_FILES]

For each task line:
- Look at the codebase and see if this has been implemented. If so, mark the item [x].
- If any two items are mutually exclusive (disregard rejected items or items marked [rej]), prompt the user which to keep and which to mark as rejected.
[IF_PERSON_FILES]- Check person-specific files for items that should be triaged into the main tracking files. Suggest which category each belongs in.[END_IF_PERSON_FILES]

After marking, call the Issue Sync hook to move everything to the correct files.
```

## Custom Category Template

When the user adds a custom category during setup:

```
Category name: [USER_INPUT]
Tag shortcode: [USER_INPUT or auto-generated from first letters]
Description: [USER_INPUT or auto-generated]
Filename: [kebab-case of category name].md
```

Validation:
- Tag must be unique across all categories
- Tag must be 2-4 lowercase letters in brackets
- Filename must not conflict with existing files
- Category name should be concise (1-3 words)
