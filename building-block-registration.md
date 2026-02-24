# Building Block Registration

**Status:** Deployed
**Type:** Augmented
**Business Process:** Operations
**Trigger:** User creates, packages, or updates a Claude building block (Skill, Agent, Prompt, or Context MD)
**Process Outcome:** Registered or updated entry in the Notion AI Building Blocks database

**Assets Used:**
- registering-building-blocks (Claude Skill)
- Notion AI Building Blocks database

---

## Overview

Register or update AI building blocks in the Notion AI Building Blocks database immediately after creating, packaging, or updating any Claude asset. Handles four asset types (Skill, Agent, Prompt, Context MD) with automatic metadata extraction and Quick Start Prompt generation. Supports both single registration and batch operations.

## Prerequisites

- [ ] Claude Code with `registering-building-blocks` skill installed
- [ ] Access to Notion AI Building Blocks database
- [ ] A building block that has been created or updated

## Trigger

**When:** After creating a new skill, agent, prompt, or context MD — or when explicitly asked to register/track a building block
**Frequency:** On-demand — runs after every build or update

---

## Procedure

### Step 1: Identify the building block (Human + AI)

Tell Claude what to register, or Claude detects the asset from context (e.g., a skill was just created in the current session).

Claude resolves the asset type using:
- Keywords in the request ("register this skill", "add this agent")
- File path patterns (`SKILL.md` → Skill, `.claude/agents/*.md` → Agent, `CLAUDE.md` → Context MD)
- If ambiguous, ask the user

### Step 2: Extract metadata (AI)

Read metadata from the appropriate source:

| Asset Type | Source | Name From | Description From |
|-----------|--------|-----------|-----------------|
| Skill | `~/.claude/skills/{name}/SKILL.md` | Frontmatter `name` | Frontmatter `description` |
| Agent | `.claude/agents/{name}.md` | Filename or frontmatter | Opening paragraph or frontmatter |
| Prompt | User-specified file or inline | User-provided | User-provided |
| Context MD | `CLAUDE.md` or specified file | User-provided or filename | User-provided or summarized |

### Step 3: Generate Quick Start Prompt (AI)

Create a single, copy-paste-ready prompt that demonstrates the building block's primary use case. Must be specific enough to be useful, generic enough to work across contexts, and produce a complete result.

> Tip: Context MDs typically don't need a Quick Start Prompt — leave blank or ask the user. If unsure about any asset type, ask rather than guess.

### Step 4: Search for existing entry (Automation)

Search the AI Building Blocks database for an exact name match. Partial or similar matches are NOT duplicates — only exact title matches count.

**Decision Point:** If an exact match exists, update it. If no match, create a new entry.

### Step 5: Create or update in Notion (Automation)

**New entry:** Create with Name, Description, Asset Type, Platform (always "Claude"), Quick Start Prompt, and GitHub URL if applicable.

**Existing entry:** Update Description and Quick Start Prompt to current values.

### Step 6: Confirm registration (AI)

Report what was created or updated, including the Notion page link.

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Metadata extraction summary | User review (in-session) | Text |
| Database entry (new or updated) | Notion AI Building Blocks database | Page with properties |
| Confirmation | Claude response | Text with action taken |

## Quality Checks

- [ ] Asset type correctly resolved (Skill, Agent, Prompt, or Context MD)
- [ ] Name matches the canonical source (frontmatter, filename, or user input)
- [ ] Quick Start Prompt is actionable and demonstrates the primary use case
- [ ] Duplicate check performed before creating — exact name match only

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Asset type is ambiguous | Ask the user which type to register. |
| Quick Start Prompt is hard to generate | Ask the user for one rather than guessing. |
| Notion search returns no results for an existing block | Check for name variations (hyphens vs. spaces, case differences). Create new if genuinely absent. |
| Batch registration with mixed types | Process each individually — search, then create or update. Report totals by type at the end. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `registering-building-blocks`
**GitHub Repository:** bengio777/agent-skills (synced via syncing-skills-to-github)

**Notion Data Source:**
- AI Building Blocks Database: `2fefb3a7-cad4-81ba-901b-000bd6f3fb6c`

**Schema Properties:** Name (title), Description (text), Asset Type (select), Platform (select), Quick Start Prompt (text), GitHub (url)
**Human Checkpoints:** Steps 1, 3 (asset identification, Quick Start Prompt review for ambiguous cases)
**Workflow Type:** Augmented (AI extracts and generates, human confirms, automation writes to Notion)
