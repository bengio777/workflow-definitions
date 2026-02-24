# AI Building Block Spec: Process Guide Writing

## Execution Pattern

**Recommended Pattern:** Skill-Powered Prompt

**Reasoning:**
- Tool use required (Notion MCP for fetching business process data, linked workflows, and saving guides) -- rules out plain Prompt
- Template-driven output with strategic framing -- reusable logic fits the Skill pattern
- Single expertise domain (process guide writing) -- doesn't need Multi-Agent
- Key AI judgment: synthesizing workflow relationships into strategic narrative -- more than deterministic, but bounded by template, so Agent is overkill
- Human review gate mid-workflow keeps it augmented

---

## Scenario Summary

| Field | Value |
|-------|-------|
| **Workflow Name** | Process Guide Writing |
| **Description** | Write Business Process Guide documentation that explains when, why, and how to execute a complete business process with its component workflows, and save to the Notion business process page body |
| **Process Outcome** | Business Process Guide saved to Notion business process page body |
| **Trigger** | User asks to document a business process, create a playbook, or explain how multiple workflows connect |
| **Type** | Augmented |
| **Business Process** | Operations |

---

## Step-by-Step Decomposition

| Step | Name | Autonomy Level | Building Block(s) | Tools / Connectors | Skill Candidate | HITL Gate |
|------|------|---------------|-------------------|-------------------|----------------|-----------|
| 1 | Identify business process | Human | -- | -- | -- | Yes -- user provides process name or link |
| 2 | Fetch process + linked workflows from Notion | AI-Deterministic | Skill | Notion MCP | `writing-process-guides` | None |
| 3 | Gather strategic context | AI-Semi-Autonomous + Human | Skill | Notion MCP | `writing-process-guides` | Yes -- user provides rhythm, frequency, decision points |
| 4 | Write Process Guide using 7-section template | AI-Semi-Autonomous | Skill | -- | `writing-process-guides` | None |
| 5 | Review draft | Human | -- | -- | -- | Yes -- user reviews for strategic accuracy |
| 6 | Save to Notion | AI-Deterministic | Skill | Notion MCP | `writing-process-guides` | None |
| 7 | Confirm save | AI-Deterministic | Skill | -- | `writing-process-guides` | None |

## Autonomy Spectrum Summary

```
|----Human----|---Semi-Autonomous---|---Deterministic---|
  Steps 1,3,5       Steps 3,4            Steps 2,6,7
```

- **3 steps involve the human** -- process identification, strategic context, draft review
- **2 steps are semi-autonomous** -- AI gathers linked workflow data and writes strategic guide
- **3 steps are deterministic** -- fetch data, save to Notion, confirm

---

## Skill Candidates

### `writing-process-guides`

**Purpose:** Write Business Process Guide documentation using the 7-section template, synthesizing linked workflow data into a strategic narrative, and save to Notion.

**Inputs:**

| Input | Required | Description |
|-------|----------|-------------|
| Business process name or Notion link | Yes | Identifies the target business process |
| Strategic context | Yes (may come from conversation) | Rhythm, frequency, timing, decision points, success criteria |

**Outputs:**

| Output | Destination | Description |
|--------|-------------|-------------|
| Process Guide draft | User (in-session) | Markdown guide for review |
| Finalized Process Guide | Notion business process page body | Saved via Notion MCP |
| Missing SOP inventory | Claude response (if applicable) | List of linked workflows that lack SOPs |
| Confirmation | Claude response | Success message with Notion link |

**Decision Logic:**

1. **Linked workflow resolution** -- Fetch all workflows linked to the business process; pull their Name, Description, Type, and Trigger
2. **SOP gap detection** -- Identify which linked workflows don't have SOPs yet; note them but proceed (Process Guide can reference "see workflow SOP for details")
3. **Strategic vs. tactical boundary** -- Keep content at the "when/why/what order" level; if detail drifts into step-by-step execution, redirect to workflow SOPs
4. **Scannability check** -- The guide should be graspable in under 2 minutes; if it exceeds 2 pages, the process may need splitting

**Failure Modes:**

| Failure | Handling |
|---------|----------|
| Business process has no linked workflows | Define workflows first using the naming-workflows skill, then return to write the guide |
| Guide drifts into tactical SOP-level detail | Pull back to strategic framing; workflow SOPs handle the "how" |
| Notion page update fails | Present the guide in a code block for manual copy-paste |
| Component workflows don't have SOPs yet | Write the guide anyway with "see workflow SOP" references; note which SOPs are missing in the output |

---

## Step Sequence and Dependencies

```
Step 1: Identify business process (Human)
    |
    v
Step 2: Fetch process + linked workflows from Notion (AI)
    |
    +-- Decision: Linked workflows exist? --No--> Define workflows first, then retry
    |
    v
Step 3: Gather strategic context (AI + Human)
    |
    +-- Decision: Context already known? --Yes--> Skip interview
    |
    v
Step 4: Write Process Guide using 7-section template (AI)
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
| 1 | User input | Business process identifier |
| 2 | Step 1 | Process properties + linked workflow data |
| 3 | Steps 1-2 + user input | Strategic context (rhythm, decisions, success criteria) |
| 4 | Steps 2-3 | Process Guide draft (Markdown) |
| 5 | Step 4 | Approved/revised guide |
| 6 | Step 5 | Notion page updated |
| 7 | Step 6 | Confirmation with link |

**All steps are sequential.** No parallelism.

---

## Prerequisites

- [ ] Claude Code with `writing-process-guides` skill installed at `~/.claude/skills/writing-process-guides/`
- [ ] Notion MCP configured with access to Business Processes and Workflows databases
- [ ] Target business process exists in Notion Business Processes database
- [ ] Component workflows identified and linked (SOPs preferred but not required)

## Context Inventory

| Artifact | Type | Used By | Status | Key Contents |
|----------|------|---------|--------|-------------|
| `SKILL.md` | Skill definition | All steps | Deployed | Process, template overview, Process Guide vs SOP distinction, writing guidelines, Notion references |
| `references/process-guide-template.md` | Template | Step 4 | Deployed | 7-section template for strategic process documentation |
| `references/api_reference.md` | Reference | Steps 2, 6 | Deployed | Notion API patterns for fetching and updating pages |
| Notion Business Processes DB | Database | Steps 2, 6 | Live | Process properties: Name, Domain, Description, linked Workflows |
| Notion Workflows DB | Database | Step 2 | Live | Workflow properties: Name, Description, Type, Trigger |

## Tools and Connectors

| Tool | Purpose | Used By Step | Integration |
|------|---------|-------------|-------------|
| **Notion MCP** | Fetch business process data, fetch linked workflows, save guide to page body | Steps 2, 6 | Configured in Claude Code |

## Recommended Implementation Order

1. **Install skill** -- Ensure `writing-process-guides` skill is in `~/.claude/skills/`
2. **Verify Notion access** -- Confirm Business Processes and Workflows databases are reachable via Notion MCP
3. **Test with a process that has linked workflows** -- Write a guide for a process with 2-3 linked workflows to validate the pipeline
4. **Test SOP gap handling** -- Run against a process where some workflows lack SOPs to confirm the missing-SOP inventory works
5. **Register in Notion** -- Add to AI Building Blocks database

## Where to Run

| Platform | Recommendation |
|----------|---------------|
| Claude Code | Primary. Skill installed at `~/.claude/skills/writing-process-guides/SKILL.md`. Runs in any Claude Code session with Notion MCP. |
| GitHub | Skill source synced to `bengio777/agent-skills` via syncing-skills-to-github. |
