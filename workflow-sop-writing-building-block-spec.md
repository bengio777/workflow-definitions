# AI Building Block Spec: Workflow SOP Writing

## Execution Pattern

**Recommended Pattern:** Skill-Powered Prompt

**Reasoning:**
- Tool use required (Notion MCP for fetching workflow data and saving SOPs) -- rules out plain Prompt
- Template-driven output with type adaptations -- reusable logic fits the Skill pattern
- Single expertise domain (SOP writing) -- doesn't need Multi-Agent
- One key AI judgment call (adapting template to workflow type) but the rest is deterministic -- doesn't need a full Agent
- Human review gate mid-workflow keeps it augmented, not fully autonomous

---

## Scenario Summary

| Field | Value |
|-------|-------|
| **Workflow Name** | Workflow SOP Writing |
| **Description** | Write comprehensive Standard Operating Procedure documentation for workflows using an 8-section template and save directly to the Notion workflow page body |
| **Process Outcome** | SOP documentation saved to Notion workflow page body |
| **Trigger** | User asks to write an SOP, document a workflow, or add operating instructions |
| **Type** | Augmented |
| **Business Process** | Operations |

---

## Step-by-Step Decomposition

| Step | Name | Autonomy Level | Building Block(s) | Tools / Connectors | Skill Candidate | HITL Gate |
|------|------|---------------|-------------------|-------------------|----------------|-----------|
| 1 | Identify workflow | Human | -- | -- | -- | Yes -- user provides workflow name or link |
| 2 | Fetch workflow context from Notion | AI-Deterministic | Skill | Notion MCP | `writing-workflow-sops` | None |
| 3 | Gather procedure details | AI-Semi-Autonomous + Human | Skill | Notion MCP | `writing-workflow-sops` | Yes -- user provides missing procedure details |
| 4 | Write SOP using 8-section template | AI-Semi-Autonomous | Skill | -- | `writing-workflow-sops` | None |
| 5 | Review draft | Human | -- | -- | -- | Yes -- user reviews for accuracy |
| 6 | Save to Notion | AI-Deterministic | Skill | Notion MCP | `writing-workflow-sops` | None |
| 7 | Confirm save | AI-Deterministic | Skill | -- | `writing-workflow-sops` | None |

## Autonomy Spectrum Summary

```
|----Human----|---Semi-Autonomous---|---Deterministic---|
  Steps 1,3,5       Steps 3,4            Steps 2,6,7
```

- **3 steps involve the human** -- workflow identification, procedure details, draft review
- **2 steps are semi-autonomous** -- AI gathers context and writes SOP with judgment calls on template adaptation
- **3 steps are deterministic** -- fetch data, save to Notion, confirm

---

## Skill Candidates

### `writing-workflow-sops`

**Purpose:** Write SOP documentation for workflows using the 8-section template, adapted for Manual/Augmented/Automated types, and save to Notion.

**Inputs:**

| Input | Required | Description |
|-------|----------|-------------|
| Workflow name or Notion link | Yes | Identifies the target workflow |
| Procedure details | Yes (may come from conversation context) | Steps, decision points, inputs/outputs, common failures |

**Outputs:**

| Output | Destination | Description |
|--------|-------------|-------------|
| SOP draft | User (in-session) | Markdown SOP for review |
| Finalized SOP | Notion workflow page body | Saved via Notion MCP |
| Confirmation | Claude response | Success message with Notion link |

**Decision Logic:**

1. **Type detection** -- Read workflow Type property from Notion (Manual, Augmented, Automated)
2. **Template adaptation** -- Adjust the 8-section template:
   - Manual: detailed human steps, time estimates, exact UI paths
   - Augmented: actor-tagged steps `(AI)`, `(Human)`, `(AI + Human)` with handoff points
   - Automated: monitoring focus, intervention points, error handling
3. **Context sufficiency** -- If procedure details are already known from conversation context, skip the interview; otherwise ask targeted questions

**Failure Modes:**

| Failure | Handling |
|---------|----------|
| Workflow doesn't exist in Notion | Create it first (or invoke naming-workflows skill), then return to write the SOP |
| Procedure details are incomplete | Ask targeted questions; if user doesn't know, write what's known and mark gaps with `[TBD]` |
| Notion page update fails | Present the SOP in a code block so the user can copy-paste manually |
| SOP is too long for a utility workflow | Apply the 80/20 rule -- troubleshooting and quality checks should cover common cases only |

---

## Step Sequence and Dependencies

```
Step 1: Identify workflow (Human)
    |
    v
Step 2: Fetch workflow context from Notion (AI)
    |
    +-- Decision: Workflow exists? --No--> Create workflow first, then retry
    |
    v
Step 3: Gather procedure details (AI + Human)
    |
    +-- Decision: Context already known? --Yes--> Skip interview
    |
    v
Step 4: Write SOP using 8-section template (AI)
    |
    v
Step 5: Review draft (Human)
    |
    +-- Feedback loop: revise and re-review as needed
    |
    v
Step 6: Save to Notion (Automation)
    |
    v
Step 7: Confirm save (AI)
```

**Dependency Map:**

| Step | Depends On | Produces |
|------|-----------|----------|
| 1 | User input | Workflow identifier |
| 2 | Step 1 | Workflow properties (Name, Type, Trigger, etc.) |
| 3 | Steps 1-2 + user input | Procedure details |
| 4 | Steps 2-3 | SOP draft (Markdown) |
| 5 | Step 4 | Approved/revised SOP |
| 6 | Step 5 | Notion page updated |
| 7 | Step 6 | Confirmation with link |

**All steps are sequential.** No parallelism.

---

## Prerequisites

- [ ] Claude Code with `writing-workflow-sops` skill installed at `~/.claude/skills/writing-workflow-sops/`
- [ ] Notion MCP configured with access to Workflows database
- [ ] Target workflow exists in Notion Workflows database (or will be created)
- [ ] Knowledge of the workflow's procedure (from user or conversation context)

## Context Inventory

| Artifact | Type | Used By | Status | Key Contents |
|----------|------|---------|--------|-------------|
| `SKILL.md` | Skill definition | All steps | Deployed | Process, template overview, type adaptations, writing guidelines, Notion references |
| `references/sop-template.md` | Template | Step 4 | Deployed | 8-section template with type-specific adaptation rules |
| Notion Workflows DB | Database | Steps 2, 6 | Live | Workflow properties: Name, Description, Type, Trigger, Apps, Assets Used |

## Tools and Connectors

| Tool | Purpose | Used By Step | Integration |
|------|---------|-------------|-------------|
| **Notion MCP** | Fetch workflow data, save SOP to page body | Steps 2, 6 | Configured in Claude Code |

## Recommended Implementation Order

1. **Install skill** -- Ensure `writing-workflow-sops` skill is in `~/.claude/skills/`
2. **Verify Notion access** -- Confirm Workflows database is reachable via Notion MCP
3. **Test with a simple workflow** -- Write an SOP for a Manual workflow to validate the pipeline
4. **Test type adaptations** -- Run against an Augmented and an Automated workflow to confirm template adjustments
5. **Register in Notion** -- Add to AI Building Blocks database

## Where to Run

| Platform | Recommendation |
|----------|---------------|
| Claude Code | Primary. Skill installed at `~/.claude/skills/writing-workflow-sops/SKILL.md`. Runs in any Claude Code session with Notion MCP. |
| GitHub | Skill source synced to `bengio777/agent-skills` via syncing-skills-to-github. |
