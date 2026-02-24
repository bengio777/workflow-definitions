# AI Building Block Spec: Skill GitHub Sync

## Execution Pattern

**Recommended Pattern:** Skill-Powered Prompt

**Reasoning:**
- Tool use required (Git CLI for status/commit/push, Notion MCP for URL updates) -- rules out plain Prompt
- Mostly deterministic pipeline (git operations, Notion updates) with one semi-autonomous step (commit message generation) -- doesn't need a full Agent
- Single expertise domain (git sync + Notion tracking) -- doesn't need Multi-Agent
- Three distinct modes (single, batch, dry run) with shared logic -- Skill pattern handles mode branching cleanly
- After human trigger, the entire pipeline runs without further intervention -- Automated type

---

## Scenario Summary

| Field | Value |
|-------|-------|
| **Workflow Name** | Skill GitHub Sync |
| **Description** | Sync Claude Agent Skills from `~/.claude/skills/` to GitHub with semantic commit messages, automated Notion tracking, and README regeneration |
| **Process Outcome** | Skills committed to GitHub with Notion AI Building Blocks database updated with GitHub URLs |
| **Trigger** | After creating or updating skills locally, or on a weekly batch schedule |
| **Type** | Automated |
| **Business Process** | Operations |

---

## Step-by-Step Decomposition

| Step | Name | Autonomy Level | Building Block(s) | Tools / Connectors | Skill Candidate | HITL Gate |
|------|------|---------------|-------------------|-------------------|----------------|-----------|
| 1 | Initiate sync | Human | -- | -- | -- | Yes -- user chooses mode (single/batch/dry run) |
| 2 | Verify git repository | AI-Deterministic | Skill | Git CLI | `syncing-skills-to-github` | None |
| 3 | Detect changes via git status | AI-Deterministic | Skill | Git CLI | `syncing-skills-to-github` | None |
| 4 | Generate semantic commit message | AI-Semi-Autonomous | Skill | Git CLI | `syncing-skills-to-github` | None |
| 5 | Stage and commit | AI-Deterministic | Skill | Git CLI | `syncing-skills-to-github` | None |
| 6 | Push to GitHub | AI-Deterministic | Skill | Git CLI | `syncing-skills-to-github` | Conditional -- only if merge conflicts |
| 7 | Update Notion with GitHub URLs | AI-Deterministic | Skill | Notion MCP | `syncing-skills-to-github` | None |
| 8 | Regenerate README | AI-Deterministic | Skill | Git CLI, Bash | `syncing-skills-to-github` | None |
| 9 | Report results | AI-Deterministic | Skill | -- | `syncing-skills-to-github` | None |

## Autonomy Spectrum Summary

```
|----Human----|---Semi-Autonomous---|---Deterministic---|
    Step 1          Step 4           Steps 2,3,5,6,7,8,9
```

- **1 step involves the human** -- mode selection and initiation
- **1 step is semi-autonomous** -- commit message generation uses judgment to describe changes semantically
- **7 steps are deterministic** -- git operations, Notion updates, README generation, reporting
- **After trigger, the pipeline is fully automated** except for conflict resolution (rare edge case)

---

## Skill Candidates

### `syncing-skills-to-github`

**Purpose:** Sync Claude Agent Skills to GitHub with semantic commit messages, push to remote, update Notion AI Building Blocks with GitHub URLs, and regenerate the skill index README.

**Inputs:**

| Input | Required | Description |
|-------|----------|-------------|
| Sync mode | Yes | Single skill name, "all" for batch, or "dry run" for preview |
| Skill name | Conditional | Required for single-skill mode only |

**Outputs:**

| Output | Destination | Description |
|--------|-------------|-------------|
| Git commit(s) | GitHub `bengio777/agent-skills` | Committed skill files with semantic messages |
| Updated GitHub URLs | Notion AI Building Blocks database | Page property updates with tree URLs |
| Updated README | GitHub `bengio777/agent-skills` | Regenerated skill index |
| Sync report | Claude response | Summary with counts (new, updated, warnings) |

**Decision Logic:**

1. **Mode selection** -- Parse user request for single skill name, "all"/"batch" keywords, or "dry run"/"preview" keywords
2. **Change detection** -- Parse `git status --porcelain` output: `??` = new, `M` = modified, `D` = deleted; filter to requested skill in single mode
3. **No-change early exit** -- If no changes detected, report last commit info and stop
4. **Semantic prefix selection** -- `[CREATE]` for new skills, `[UPDATE]` for modified, `[FIX]` for corrections, `[SYNC]` for batch, `[RETIRE]` for archived, `[REFACTOR]` for restructured, `[DOCS]` for docs-only changes
5. **Push failure handling** -- If push rejected due to divergence, pull with rebase first; if merge conflicts, report and ask user to resolve
6. **Notion miss handling** -- If a skill isn't found in Notion, log warning and continue with others; suggest registering via `registering-building-blocks`

**Failure Modes:**

| Failure | Handling |
|---------|----------|
| Not a git repository | Provide initialization commands (`git init`, `git remote add origin`, initial commit and push) and stop |
| Push rejected (branch diverged) | Pull with rebase first (`git pull --rebase origin main && git push`). If conflicts, report and ask user to resolve. |
| Merge conflicts | Report conflicting files with resolution instructions. Do not force push. |
| Notion skill not found | Log warning, continue with other skills. Suggest registering the missing skill. |
| No changes detected | Report last sync info (commit hash, date, message) and exit cleanly |

---

## Step Sequence and Dependencies

```
Step 1: Initiate sync (Human)
    |
    v
Step 2: Verify git repository (Automation)
    |
    +-- Decision: Valid repo? --No--> Report init steps, stop
    |
    v
Step 3: Detect changes via git status (Automation)
    |
    +-- Decision: Changes found? --No--> Report "already synced", stop
    |
    +-- Dry run mode? --Yes--> Show preview, generate message, stop
    |
    v
Step 4: Generate semantic commit message (AI)
    |
    v
Step 5: Stage and commit (Automation)
    |
    v
Step 6: Push to GitHub (Automation)
    |
    +-- Decision: Push succeeded? --No--> Pull rebase, retry
    |   +-- Conflicts? --Yes--> Report, ask user, stop
    |
    v
Step 7: Update Notion with GitHub URLs (Automation)
    |
    +-- Per-skill: search, update URL + status. Log warnings for misses.
    |
    v
Step 8: Regenerate README (Automation)
    |
    v
Step 9: Report results (AI)
```

**Dependency Map:**

| Step | Depends On | Produces |
|------|-----------|----------|
| 1 | User input | Sync mode + optional skill name |
| 2 | Step 1 | Git repo validation |
| 3 | Step 2 | List of changed skills |
| 4 | Step 3 | Semantic commit message |
| 5 | Steps 3-4 | Git commit |
| 6 | Step 5 | Remote push |
| 7 | Step 6 | Notion page updates |
| 8 | Step 6 | Updated README committed and pushed |
| 9 | Steps 6-8 | Summary report |

**Steps are sequential.** Steps 7 and 8 could theoretically parallelize but are run sequentially for simplicity.

---

## Prerequisites

- [ ] Claude Code running in terminal with git credentials configured
- [ ] `~/.claude/skills/` initialized as a git repository with remote `bengio777/agent-skills`
- [ ] Notion MCP configured with access to AI Building Blocks database
- [ ] `generate-readme.sh` script present in `~/.claude/skills/`

## Context Inventory

| Artifact | Type | Used By | Status | Key Contents |
|----------|------|---------|--------|-------------|
| `SKILL.md` | Skill definition | All steps | Deployed | Prerequisites, implementation steps, usage modes, error handling, commit prefixes, Notion update instructions |
| `references/QUICK-REFERENCE.md` | Reference | Quick lookup | Deployed | Condensed command reference for common sync operations |
| GitHub repo `bengio777/agent-skills` | Repository | Steps 5-6, 8 | Live | Remote target for skill files |
| Notion AI Building Blocks DB | Database | Step 7 | Live | Properties: Name, GitHub (url), Status (select) |
| `generate-readme.sh` | Script | Step 8 | Deployed | Scans skill folders, extracts descriptions, generates README table |

## Tools and Connectors

| Tool | Purpose | Used By Step | Integration |
|------|---------|-------------|-------------|
| **Git CLI** | Status, add, commit, push, pull, log | Steps 2, 3, 5, 6, 8 | Terminal access in Claude Code |
| **Notion MCP** | Search for skills, update GitHub URL and Status properties | Step 7 | Configured in Claude Code |
| **Bash** | Run `generate-readme.sh` | Step 8 | Terminal access in Claude Code |

## Recommended Implementation Order

1. **Install skill** -- Ensure `syncing-skills-to-github` skill is in `~/.claude/skills/`
2. **Verify git setup** -- Confirm `~/.claude/skills/` is a git repo with correct remote
3. **Test single-skill sync** -- Modify one skill and sync to validate the full pipeline
4. **Test batch sync** -- Make changes to 2-3 skills and run batch mode
5. **Test dry run** -- Verify preview mode shows correct changes without committing
6. **Test error paths** -- Verify behavior for no-changes, push rejection, and Notion miss scenarios
7. **Register in Notion** -- Add to AI Building Blocks database

## Where to Run

| Platform | Recommendation |
|----------|---------------|
| Claude Code | Primary. Skill installed at `~/.claude/skills/syncing-skills-to-github/SKILL.md`. Requires terminal access with git credentials. Runs in any Claude Code session with Git + Notion MCP. |
| GitHub | Skill source is self-hosted in `bengio777/agent-skills` (the same repo it syncs to). |
