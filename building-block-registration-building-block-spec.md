# AI Building Block Spec: Building Block Registration

## Execution Pattern

**Recommended Pattern:** Skill-Powered Prompt

**Reasoning:**
- Tool use required (Notion MCP for searching, creating, and updating database entries) -- rules out plain Prompt
- Deterministic metadata extraction + semi-autonomous Quick Start Prompt generation -- bounded judgment, doesn't need a full Agent
- Single expertise domain (asset registration) -- doesn't need Multi-Agent
- Reusable asset-type resolution and registration logic -- Skill pattern fits
- Supports batch operations with mixed types -- skill handles iteration internally

---

## Scenario Summary

| Field | Value |
|-------|-------|
| **Workflow Name** | Building Block Registration |
| **Description** | Register or update AI building blocks (Skills, Agents, Prompts, Context MDs) in the Notion AI Building Blocks database with automatic metadata extraction and Quick Start Prompt generation |
| **Process Outcome** | Registered or updated entry in the Notion AI Building Blocks database |
| **Trigger** | After creating, packaging, or updating a Claude building block (Skill, Agent, Prompt, Context MD) |
| **Type** | Augmented |
| **Business Process** | Operations |

---

## Step-by-Step Decomposition

| Step | Name | Autonomy Level | Building Block(s) | Tools / Connectors | Skill Candidate | HITL Gate |
|------|------|---------------|-------------------|-------------------|----------------|-----------|
| 1 | Identify building block | AI-Semi-Autonomous + Human | Skill | File system | `registering-building-blocks` | Conditional -- only if asset type is ambiguous |
| 2 | Extract metadata from source | AI-Deterministic | Skill | File system | `registering-building-blocks` | None |
| 3 | Generate Quick Start Prompt | AI-Semi-Autonomous | Skill | -- | `registering-building-blocks` | Conditional -- ask user if unsure |
| 4 | Search for existing entry | AI-Deterministic | Skill | Notion MCP | `registering-building-blocks` | None |
| 5 | Create or update in Notion | AI-Deterministic | Skill | Notion MCP | `registering-building-blocks` | None |
| 6 | Confirm registration | AI-Deterministic | Skill | -- | `registering-building-blocks` | None |

## Autonomy Spectrum Summary

```
|----Human----|---Semi-Autonomous---|---Deterministic---|
  Steps 1,3        Steps 1,3           Steps 2,4,5,6
  (conditional)
```

- **4 steps are deterministic** -- metadata extraction, Notion search, create/update, confirm
- **2 steps are semi-autonomous** -- asset type resolution and Quick Start Prompt generation involve judgment
- **Human involvement is conditional** -- only triggered when asset type is ambiguous or Quick Start Prompt is unclear

---

## Skill Candidates

### `registering-building-blocks`

**Purpose:** Register or update AI building blocks in the Notion AI Building Blocks database with automatic metadata extraction, Quick Start Prompt generation, and duplicate detection.

**Inputs:**

| Input | Required | Description |
|-------|----------|-------------|
| Building block identifier | Yes | Name, file path, or conversation context identifying the asset |
| Asset type | Conditional | Skill, Agent, Prompt, or Context MD -- resolved automatically or asked |

**Outputs:**

| Output | Destination | Description |
|--------|-------------|-------------|
| Metadata extraction summary | User (in-session) | Name, description, asset type, Quick Start Prompt |
| Database entry | Notion AI Building Blocks database | New page or updated properties |
| Confirmation | Claude response | Action taken (created/updated) with page link |

**Decision Logic:**

1. **Asset type resolution** -- Resolve from keywords in user request, file path patterns (`SKILL.md` -> Skill, `.claude/agents/*.md` -> Agent, `CLAUDE.md` -> Context MD), or ask user if ambiguous
2. **Metadata extraction** -- Read name and description from source:
   - Skill: `SKILL.md` frontmatter `name` and `description`
   - Agent: filename or frontmatter + opening paragraph
   - Prompt: user-provided
   - Context MD: user-provided or filename
3. **Quick Start Prompt generation** -- One sentence, copy-paste-ready, demonstrates primary use case. Context MDs typically skip this.
4. **Duplicate detection** -- Exact name match only. Partial or similar names are NOT duplicates.
5. **Create vs. update** -- Exact match found -> update Description and Quick Start Prompt. No match -> create new entry with all properties.

**Failure Modes:**

| Failure | Handling |
|---------|----------|
| Asset type is ambiguous | Ask the user which type to register |
| Quick Start Prompt is hard to generate | Ask the user for one rather than guessing |
| Notion search returns no results for an existing block | Check for name variations (hyphens vs. spaces, case differences). Create new if genuinely absent. |
| Batch registration with mixed types | Process each individually -- search, then create or update. Report totals by type at the end. |

---

## Step Sequence and Dependencies

```
Step 1: Identify building block (Human + AI)
    |
    +-- Decision: Asset type clear? --No--> Ask user
    |
    v
Step 2: Extract metadata from source (AI)
    |
    v
Step 3: Generate Quick Start Prompt (AI)
    |
    +-- Decision: Confident in prompt? --No--> Ask user
    |
    v
Step 4: Search for existing entry (Automation)
    |
    +-- Decision: Exact match found? --Yes--> Update existing
    |                                  --No---> Create new
    v
Step 5: Create or update in Notion (Automation)
    |
    v
Step 6: Confirm registration (AI)
```

**Dependency Map:**

| Step | Depends On | Produces |
|------|-----------|----------|
| 1 | User input + file system | Asset type + identifier |
| 2 | Step 1 | Name, description, GitHub URL |
| 3 | Steps 1-2 | Quick Start Prompt |
| 4 | Step 2 (name) | Existing page ID or null |
| 5 | Steps 2-4 | Notion page (created or updated) |
| 6 | Step 5 | Confirmation with action taken |

**Batch mode:** Steps 1-6 repeat per building block. Report aggregated totals after all items processed.

---

## Prerequisites

- [ ] Claude Code with `registering-building-blocks` skill installed at `~/.claude/skills/registering-building-blocks/`
- [ ] Notion MCP configured with access to AI Building Blocks database
- [ ] A building block that has been created or updated (source file accessible)

## Context Inventory

| Artifact | Type | Used By | Status | Key Contents |
|----------|------|---------|--------|-------------|
| `SKILL.md` | Skill definition | All steps | Deployed | Asset type resolution rules, schema, process, batch registration instructions, Quick Start Prompt guidelines |
| Notion AI Building Blocks DB | Database | Steps 4, 5 | Live | Properties: Name, Description, Asset Type, Platform, Quick Start Prompt, GitHub |

## Tools and Connectors

| Tool | Purpose | Used By Step | Integration |
|------|---------|-------------|-------------|
| **Notion MCP** | Search for existing entries, create new pages, update existing pages | Steps 4, 5 | Configured in Claude Code |
| **File system** | Read SKILL.md frontmatter, agent files, CLAUDE.md | Steps 1, 2 | Local file access in Claude Code |

## Recommended Implementation Order

1. **Install skill** -- Ensure `registering-building-blocks` skill is in `~/.claude/skills/`
2. **Verify Notion access** -- Confirm AI Building Blocks database is reachable via Notion MCP
3. **Test single registration** -- Register a Skill to validate the full pipeline
4. **Test each asset type** -- Register an Agent, Prompt, and Context MD to confirm type resolution works
5. **Test batch registration** -- Register 2-3 mixed-type assets in one invocation
6. **Test duplicate detection** -- Re-register an existing asset to confirm update (not duplicate) behavior

## Where to Run

| Platform | Recommendation |
|----------|---------------|
| Claude Code | Primary. Skill installed at `~/.claude/skills/registering-building-blocks/SKILL.md`. Runs in any Claude Code session with Notion MCP. |
| GitHub | Skill source synced to `bengio777/agent-skills` via syncing-skills-to-github. |
