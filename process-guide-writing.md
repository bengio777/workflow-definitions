# Process Guide Writing

**Status:** Deployed
**Type:** Augmented
**Business Process:** Operations
**Trigger:** User asks to document a business process, create a playbook, or explain how multiple workflows connect
**Process Outcome:** Business Process Guide saved to Notion business process page body

**Assets Used:**
- writing-process-guides (Claude Skill)
- references/process-guide-template.md (7-section template)
- Notion Business Processes database
- Notion Workflows database

---

## Overview

Write Business Process Guide documentation that explains the strategic context, timing, and rhythm of a complete business process and its component workflows. Process Guides answer "when, why, and what order" — individual workflow SOPs handle the tactical "how." Saves directly to the Notion business process page body.

## Prerequisites

- [ ] Claude Code with `writing-process-guides` skill installed
- [ ] Target business process exists in Notion Business Processes database
- [ ] Component workflows identified and linked (SOPs preferred but not required)

## Trigger

**When:** User asks to "write a process guide," "document this process," "create a playbook," or "how do these workflows connect?"
**Frequency:** On-demand — typically after the component workflows are defined

---

## Procedure

### Step 1: Identify the business process (Human)

Provide the business process name, a Notion link, or describe which process to document.

### Step 2: Fetch process and linked workflows from Notion (AI)

Pull the business process properties (Name, Domain, Description) and all linked workflows with their sequence, triggers, and descriptions.

**Decision Point:** If linked workflows don't have SOPs yet, note which ones need them. The Process Guide can reference "see workflow SOP for details" — SOPs don't have to exist first, but they should be written eventually.

### Step 3: Gather strategic context (AI + Human)

Ask about elements that aren't captured in the workflow data:
- What's the rhythm and frequency of this process?
- Where are the key decision points between workflows?
- What does success look like for the overall process (not individual workflows)?

> Tip: Process Guides are 1-2 pages. If the answer to every question is "it depends," the process may need to be split.

### Step 4: Write the Process Guide (AI)

Generate the guide using the 7-section template:

| Section | Purpose |
|---------|---------|
| Purpose | Why this process exists and business impact |
| When to Execute | Triggers, frequency, timing |
| Process Overview | Visual flow of workflows |
| Workflow Sequence | Each workflow with trigger, duration, output |
| Decision Points | Key choices during the process |
| Success Criteria | How to know the process worked |
| Common Pitfalls | What typically goes wrong |

Focus on the "why" and "when" — keep it scannable so someone grasps the entire process in under 2 minutes.

### Step 5: Review the draft (Human)

Review for strategic accuracy, workflow sequencing, and decision point completeness. Flag missing context or incorrect ordering.

### Step 6: Save to Notion (Automation)

Update the business process page body in Notion with the finalized guide content.

### Step 7: Confirm save (AI)

Report success with a link to the updated Notion page.

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Process Guide draft | User review (in-session) | Markdown |
| Finalized Process Guide | Notion business process page body | Markdown |
| Missing SOP inventory (if applicable) | Claude response | List of workflows needing SOPs |
| Confirmation | Claude response | Text with Notion link |

## Quality Checks

- [ ] Guide is scannable in under 2 minutes — strategic, not tactical
- [ ] Workflow sequence matches actual execution order with correct triggers
- [ ] Decision points documented with clear branching criteria
- [ ] Success criteria are measurable and process-level (not workflow-level)

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Business process has no linked workflows | Define the workflows first using the naming-workflows skill, then return to write the guide. |
| Guide keeps drifting into tactical SOP-level detail | Pull back to strategic framing. Workflow SOPs handle the "how" — the guide handles "when, why, what order." |
| Notion page update fails | Present the guide in a code block for manual copy-paste. |
| Component workflows don't have SOPs yet | Write the guide anyway with "see workflow SOP" references. Note which SOPs are missing in the output. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `writing-process-guides`
**GitHub Repository:** bengio777/agent-skills (synced via syncing-skills-to-github)

**Notion Data Sources:**
1. Business Processes Database: `collection://2fefb3a7-cad4-813b-9551-000bc8a3526f`
2. Workflows Database: `collection://2fefb3a7-cad4-81f9-bf72-000b420f1b66`

**Template:** `references/process-guide-template.md` (7-section strategic template)
**Human Checkpoints:** Steps 1, 3, 5 (process identification, strategic context, draft review)
**Workflow Type:** Augmented (AI writes, human reviews, automation saves to Notion)
