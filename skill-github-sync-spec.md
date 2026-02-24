# Skill GitHub Sync -- Project Spec

**Project:** Skill GitHub Sync

---

## Capabilities Demonstrated

### 1. Automated Git Operations Pipeline
**Met.** The skill executes a full git pipeline -- status detection, staging, semantic commit, push -- without human intervention after the initial trigger. All git commands are templated in the SKILL.md with explicit error handling for each step.

**Evidence:** `~/.claude/skills/syncing-skills-to-github/SKILL.md` (Implementation Steps 1-6 with bash command templates)

### 2. Semantic Commit Message Generation
**Met.** The skill generates commit messages with semantic prefixes ([CREATE], [UPDATE], [FIX], [REFACTOR], [DOCS], [SYNC], [RETIRE]) based on the type of change detected. Single-skill and batch commit message formats are distinct and well-defined.

**Evidence:** `~/.claude/skills/syncing-skills-to-github/SKILL.md` (Step 4: Generate Commit Message with prefix table and format templates for single and batch modes)

### 3. Three Execution Modes
**Met.** The skill supports single-skill sync (targeted), batch sync (all changes), and dry run (preview without committing). Each mode has a distinct trigger phrase, process flow, and example interaction.

**Evidence:** `~/.claude/skills/syncing-skills-to-github/SKILL.md` (Usage Modes section with Mode 1, 2, and 3 fully specified with triggers, processes, and example dialogues)

### 4. Cross-System Integration (Git + Notion)
**Met.** After pushing to GitHub, the skill updates the Notion AI Building Blocks database with GitHub tree URLs and sets Status to "Deployed" for each synced skill. Missing entries are logged as warnings without blocking the sync.

**Evidence:** `~/.claude/skills/syncing-skills-to-github/SKILL.md` (Step 7: Update Notion AI Building Blocks with search, URL construction, and update instructions)

### 5. Automated README Generation
**Met.** The skill runs `generate-readme.sh` after each sync to regenerate the skill index, then commits and pushes the updated README as a separate `[DOCS]` commit.

**Evidence:** `~/.claude/skills/syncing-skills-to-github/SKILL.md` (Step 8: Generate README.md)

### 6. Error Handling for External Systems
**Met.** Five distinct error scenarios are handled: not a git repo, push rejected (diverged), merge conflicts, Notion skill not found, and no changes detected. Each has specific recovery steps.

**Evidence:** `~/.claude/skills/syncing-skills-to-github/SKILL.md` (Error Handling section with 5 scenarios, each with error message template and resolution steps), `~/Projects/workflow-definitions/skill-github-sync.md` (When Things Go Wrong table)

---

## Deliverables

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Skill definition | `~/.claude/skills/syncing-skills-to-github/SKILL.md` | Complete |
| 2 | Quick reference | `~/.claude/skills/syncing-skills-to-github/references/QUICK-REFERENCE.md` | Complete |
| 3 | Workflow definition | `~/Projects/workflow-definitions/skill-github-sync.md` | Complete |
| 4 | Building block spec | `~/Projects/workflow-definitions/skill-github-sync-building-block-spec.md` | Complete |
| 5 | Project spec | `~/Projects/workflow-definitions/skill-github-sync-spec.md` | Complete |

---

## Review Criteria

| Criterion | Status | Evidence |
|-----------|--------|----------|
| Skill executes end-to-end without errors | **Pass** | Deployed and in active use. Skills regularly synced to `bengio777/agent-skills` with Notion tracking updated. |
| Semantic commit messages are correct | **Pass** | `SKILL.md` defines 7 semantic prefixes with usage rules. Single-skill and batch message formats are templated. |
| All three modes work | **Pass** | `SKILL.md` Usage Modes section specifies triggers, processes, and example dialogues for single, batch, and dry run modes. |
| Notion updates succeed with correct URLs | **Pass** | `SKILL.md` Step 7 constructs tree URLs in format `https://github.com/bengio777/agent-skills/tree/main/{skill-name}` and updates via `notion-update-page`. |
| README regeneration runs after sync | **Pass** | `SKILL.md` Step 8 runs `generate-readme.sh`, stages, commits with `[DOCS]` prefix, and pushes. |
| Error scenarios handled gracefully | **Pass** | 5 error scenarios in `SKILL.md` Error Handling section: not a git repo, push rejected, merge conflicts, Notion miss, no changes. Each has specific recovery. |
| Part 2 of two-skill workflow documented | **Pass** | `SKILL.md` Integration Points section documents the Cloud -> Local -> GitHub pipeline and relationship to `exporting-skills-from-claude`. |

---

## File Inventory

| File | Location | Purpose |
|------|----------|---------|
| `SKILL.md` | `~/.claude/skills/syncing-skills-to-github/` | Skill definition with prerequisites, implementation steps, usage modes, error handling, commit prefixes, Notion update instructions, and integration points |
| `QUICK-REFERENCE.md` | `~/.claude/skills/syncing-skills-to-github/references/` | Condensed command reference for common sync operations |
| `skill-github-sync.md` | `~/Projects/workflow-definitions/` | Workflow definition (SOP for the sync workflow) |
| `skill-github-sync-building-block-spec.md` | `~/Projects/workflow-definitions/` | AI Building Block Spec with step decomposition and autonomy mapping |
| `skill-github-sync-spec.md` | `~/Projects/workflow-definitions/` | This file -- project spec with capabilities, deliverables, and review criteria |
