# Workflow SOP Writing

**Status:** Deployed
**Type:** Augmented
**Business Process:** Operations
**Trigger:** User asks to write an SOP, document a workflow, or add operating instructions to a workflow
**Process Outcome:** SOP documentation saved to Notion workflow page body

**Assets Used:**
- writing-workflow-sops (Claude Skill)
- references/sop-template.md (8-section template)
- Notion Workflows database

---

## Overview

Write comprehensive Standard Operating Procedure documentation for workflows and save directly to the Notion workflow page body. Adapts the 8-section template for Manual, Augmented, and Automated workflow types — emphasizing human steps for Manual, actor-tagged handoffs for Augmented, and monitoring/intervention points for Automated.

## Prerequisites

- [ ] Claude Code with `writing-workflow-sops` skill installed
- [ ] Target workflow exists in Notion Workflows database (or will be created)
- [ ] Knowledge of the workflow's procedure (from user or from prior conversation context)

## Trigger

**When:** User asks to write an SOP, document a workflow, or says "add instructions to this workflow"
**Frequency:** On-demand — runs whenever a workflow needs documentation

---

## Procedure

### Step 1: Identify the workflow (Human)

Provide the workflow name, a Notion link, or describe which workflow to document.

### Step 2: Fetch workflow context from Notion (AI)

Pull the workflow's properties: Name, Description, Type (Manual/Augmented/Automated), Trigger, Apps, and Assets Used. These inform the SOP's structure and tone.

**Decision Point:** If the workflow doesn't exist in Notion yet, create it first (or invoke the naming-workflows skill).

### Step 3: Gather procedure details (AI + Human)

If the procedure isn't already known from conversation context, ask clarifying questions:
- What are the steps from start to finish?
- Where are the decision points?
- What are the inputs and outputs?
- What commonly goes wrong?

> Tip: If a workflow was just built in the current session, Claude already has the context — skip the interview and draft directly.

### Step 4: Write the SOP (AI)

Generate the SOP using the 8-section template adapted for the workflow's Type:

| Section | Purpose |
|---------|---------|
| Overview | 1-2 sentence summary |
| Prerequisites | Access, data, tools needed |
| Trigger | When/how workflow starts |
| Procedure | Step-by-step instructions with actor tags |
| Outputs | Deliverables with destinations |
| Quality Checks | Success verification |
| When Things Go Wrong | Common problems + fixes |
| Automation Notes | Platform, skills, human checkpoints |

**Type adaptations:**
- **Manual** — Detailed human steps, time estimates, exact UI paths
- **Augmented** — Steps tagged `(AI)`, `(Human)`, or `(AI + Human)` with handoff points
- **Automated** — Focus on monitoring, intervention points, error handling

### Step 5: Review the draft (Human)

Review the SOP for accuracy, completeness, and tone. Flag anything that's missing, wrong, or over-documented.

### Step 6: Save to Notion (Automation)

Update the workflow page body in Notion with the finalized SOP content.

### Step 7: Confirm save (AI)

Report success with a link to the updated Notion page.

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| SOP draft | User review (in-session) | Markdown |
| Finalized SOP | Notion workflow page body | Markdown |
| Confirmation | Claude response | Text with Notion link |

## Quality Checks

- [ ] SOP follows the 8-section template structure
- [ ] Procedure steps start with action verbs — one action per step
- [ ] Actor tags applied correctly for Augmented workflows
- [ ] Decision points documented as explicit branches
- [ ] Tips reserved for non-obvious gotchas only

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Workflow doesn't exist in Notion | Create it first using the naming-workflows skill, then return to write the SOP. |
| Procedure details are incomplete | Ask targeted questions. If the user doesn't know yet, write what's known and mark gaps with [TBD]. |
| Notion page update fails | Present the SOP in a code block so the user can copy-paste it manually. |
| SOP is too long for a utility workflow | Apply the 80/20 rule — troubleshooting and quality checks should cover common cases only. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `writing-workflow-sops`
**GitHub Repository:** bengio777/agent-skills (synced via syncing-skills-to-github)

**Notion Data Source:**
- Workflows Database: `collection://2fefb3a7-cad4-81f9-bf72-000b420f1b66`

**Template:** `references/sop-template.md` (8-section template with type-specific adaptations)
**Human Checkpoints:** Steps 1, 3, 5 (workflow identification, procedure details, draft review)
**Workflow Type:** Augmented (AI writes, human reviews, automation saves to Notion)
