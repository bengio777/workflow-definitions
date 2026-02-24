# Workflow Naming

**Status:** Deployed
**Type:** Augmented
**Business Process:** Operations
**Trigger:** User needs to name a workflow, write a workflow description, or add a workflow to Notion
**Process Outcome:** Named workflow entry in the Notion Workflows database

**Assets Used:**
- naming-workflows (Claude Skill)
- Notion Workflows database
- Notion Business Processes database

---

## Overview

Generate consistent, outcome-focused names and descriptions for business workflows using domain-specific naming patterns, then create or update entries in the Notion Workflows database. Enforces a 2-4 word noun phrase convention with structured descriptions and tangible process outcomes.

## Prerequisites

- [ ] Claude Code with `naming-workflows` skill installed
- [ ] Access to Notion Workflows database
- [ ] Access to Notion Business Processes database

## Trigger

**When:** User describes a new workflow, asks to name a workflow, or wants to add a workflow to the Notion registry
**Frequency:** On-demand — runs whenever a new workflow is identified or documented

---

## Procedure

### Step 1: Describe the workflow (Human)

Tell Claude what the workflow does. This can be a rough description, a formal specification, or just a sentence or two about the business function.

### Step 2: Identify domain and pattern (AI)

Determine the workflow's business domain (Sales, Marketing, Product, Education, Consulting, Operations, Finance) and match it to the appropriate naming pattern:

| Domain | Pattern | Example |
|--------|---------|---------|
| Sales | [Prospect Type] [Action] | Lead Qualification |
| Marketing | [Content Type] [Action/Purpose] | Content Repurposing |
| Product | [Deliverable] [Action] | Lesson Content Creation |
| Education | [Student/Cohort] [Activity] | Student Onboarding |
| Operations | [Function] [Process] | Email Response Drafting |
| Finance | [Transaction Type] [Action] | Invoice Generation |

### Step 3: Generate name options (AI)

Present 2-3 name options using the domain pattern. Each name must be:
- 2-4 words maximum
- A noun phrase (not a verb phrase)
- Title case
- Self-explanatory without context

> Tip: Avoid anti-patterns — no verb phrases ("Managing Email"), no tool names ("Claude Email Tool"), no jargon without context ("SOP-001").

### Step 4: Write description and process outcome (AI)

For each name option, generate:
- **Description:** 1-2 sentences following the pattern `[Action verb] [object] [condition]. [Outcome].`
- **Process Outcome:** A concrete deliverable in 2-5 words (not "workflow completed" — the tangible thing produced)

### Step 5: Select and confirm (Human)

Review the options. Pick a name, description, and process outcome — or modify any of them.

### Step 6: Link to business process (AI + Human)

Search Notion Business Processes database for the appropriate parent process. Present options for user confirmation. Determine the sequence number by querying existing workflows under that process and assigning the next available multiple of 10.

**Decision Point:** If no matching business process exists, ask whether to create one or leave the workflow unlinked.

### Step 7: Create entry in Notion (Automation)

Create the workflow entry in the Notion Workflows database with all properties: Name, Description, Process Outcome, Business Process (relation), Sequence, Status (default: Under Development), Type, and Trigger.

### Step 8: Confirm creation (AI)

Report the created entry with a link to the Notion page.

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Named workflow with description | User review (in-session) | 2-3 options |
| Workflow entry | Notion Workflows database | Page with properties |
| Confirmation | Claude response | Text with Notion link |

## Quality Checks

- [ ] Name is 2-4 words, noun phrase, title case
- [ ] Description follows the action + outcome pattern (1-2 sentences)
- [ ] Process outcome is a tangible deliverable, not "completed" or "done"
- [ ] Sequence number uses multiples of 10 and doesn't conflict with existing workflows

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Name exceeds 4 words | Simplify — find the core noun phrase. If the concept needs more words, it may be two workflows. |
| No business process match in Notion | Ask user whether to create a new process or leave unlinked. |
| Duplicate workflow name in database | Search first. If a match exists, ask whether to update the existing entry or differentiate the new one. |
| User can't articulate the workflow | Ask: "What triggers this? What's the output? Who does it?" Build from those three answers. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `naming-workflows`
**GitHub Repository:** bengio777/agent-skills (synced via syncing-skills-to-github)

**Notion Data Sources:**
1. Workflows Database: `2fefb3a7-cad4-81f9-bf72-000b420f1b66`
2. Business Processes Database: `2fefb3a7-cad4-813b-9551-000bc8a3526f`

**Human Checkpoints:** Steps 1, 5, 6 (workflow description, name selection, business process confirmation)
**Workflow Type:** Augmented (AI generates, human selects, automation writes to Notion)
