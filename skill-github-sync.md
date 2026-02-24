# Skill GitHub Sync

**Status:** Deployed
**Type:** Automated
**Business Process:** Operations
**Trigger:** After creating or updating skills locally, or on a weekly batch schedule
**Process Outcome:** Skills committed to GitHub with Notion tracking updated

**Assets Used:**
- syncing-skills-to-github (Claude Skill)
- GitHub repository: bengio777/agent-skills
- Notion AI Building Blocks database

---

## Overview

Sync Claude Agent Skills from `~/.claude/skills/` to the GitHub repository `bengio777/agent-skills` with semantic commit messages and automated Notion tracking. Part 2 of the two-skill export-to-sync workflow. Supports single-skill sync, batch sync, and dry-run preview modes.

## Prerequisites

- [ ] Claude Code running in terminal with git credentials configured
- [ ] `~/.claude/skills/` initialized as a git repository with remote `bengio777/agent-skills`
- [ ] Access to Notion AI Building Blocks database

## Trigger

**When:** After creating or updating skills locally, after exporting from cloud, or during weekly batch sync
**Frequency:** On-demand (after each skill change) or weekly (Sunday batch)

---

## Procedure

### Step 1: Initiate sync (Human)

Choose a mode:
- **Single skill:** "Sync the [skill-name] skill to GitHub"
- **Batch sync:** "Sync all changed skills to GitHub"
- **Dry run:** "Show what would sync to GitHub"

### Step 2: Verify git repository (Automation)

Confirm `~/.claude/skills/` is a git repo with the correct remote. If not, report the initialization steps needed and stop.

### Step 3: Detect changes (Automation)

Run `git status` to identify:
- New skills (untracked directories with `SKILL.md`)
- Modified skills (changed `SKILL.md` or reference files)
- Deleted skills (removed directories)

For single-skill mode, filter to just the requested skill.

**Decision Point:** If no changes detected, report "everything is already synced" with the last commit info and stop.

### Step 4: Generate semantic commit message (Automation)

**Single skill:**
```
[CREATE|UPDATE|FIX] skill-name: Brief description
```

**Batch:**
```
[SYNC] Updated N skills

New skills:
- skill-a: Description

Updated skills:
- skill-b: What changed
```

> Tip: Prefixes are semantic — `[CREATE]` for new, `[UPDATE]` for modified, `[FIX]` for corrections, `[SYNC]` for batch, `[RETIRE]` for archived.

### Step 5: Stage and commit (Automation)

Stage the changes and commit with the generated message. Verify with `git log -1`.

### Step 6: Push to GitHub (Automation)

Push to `origin main`. If push fails due to branch divergence, pull with rebase first, then push.

**Decision Point:** If merge conflicts arise, report them and ask the user to resolve manually.

### Step 7: Update Notion AI Building Blocks (Automation)

For each synced skill:
1. Search the AI Building Blocks database for the skill name
2. Update the GitHub property with the tree URL: `https://github.com/bengio777/agent-skills/tree/main/{skill-name}`
3. Set Status to "Deployed"

If a skill isn't found in Notion, log a warning and continue with the others.

### Step 8: Regenerate README (Automation)

Run `generate-readme.sh` to update the skill index. Stage, commit, and push the updated README.

### Step 9: Report results (AI)

Summary: X skills synced (Y new, Z updated), Notion entries updated, any warnings or failures.

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Committed skills | GitHub bengio777/agent-skills | Git commit |
| Updated GitHub URLs | Notion AI Building Blocks database | Page property updates |
| Updated README | GitHub bengio777/agent-skills | Markdown index |
| Sync report | Claude response | Summary with counts |

## Quality Checks

- [ ] Commit messages use semantic prefixes and describe what changed
- [ ] All changed skills are captured — none missed in batch mode
- [ ] Notion GitHub URLs point to correct tree paths
- [ ] README reflects current skill inventory after sync

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Not a git repository | Provide initialization commands: `git init`, `git remote add origin`, initial commit and push. |
| Push rejected (branch diverged) | Pull with rebase first: `git pull --rebase origin main && git push`. If conflicts, report and ask user to resolve. |
| Notion skill not found | Log warning, continue with others. Suggest registering the missing skill using the registering-building-blocks skill. |
| No changes detected | Report last sync info and exit cleanly. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `syncing-skills-to-github`
**GitHub Repository:** bengio777/agent-skills

**Integration Points:**
- Part 2 of two-skill workflow: Cloud → Local (exporting-skills-from-claude) → GitHub (this skill)
- Notion AI Building Blocks updated with GitHub URLs and Deployed status after each sync

**Notion Data Source:**
- AI Building Blocks Database: `2fefb3a7-cad4-81ba-901b-000bd6f3fb6c`

**Sync Modes:** Single skill, batch (all changes), dry run (preview only)
**Human Checkpoints:** Step 1 only (mode selection and initiation)
**Workflow Type:** Automated (fully automated after trigger, except conflict resolution)
