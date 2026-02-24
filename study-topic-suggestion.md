# Study Topic Suggestion

**Status:** Deployed
**Type:** Automated
**Business Process:** Education
**Trigger:** User asks what to study next or requests coverage stats
**Process Outcome:** Prioritized study topic recommendation with reasoning

**Assets Used:**
- suggest-next-topic (Claude Skill)
- Notion Security+ SY0-701 Progress Tracker (Sub-Topics DB + Objectives DB)

---

## Overview

Query the Security+ SY0-701 progress tracker in Notion and recommend the next study topic using a five-tier priority system. Factors in mastery level, deck creation status, cross-topic reinforcement, and exam ROI to surface the highest-value next action.

## Prerequisites

- [ ] Claude Code with `suggest-next-topic` skill installed
- [ ] Notion "Security+ SY0-701 Progress Tracker" databases accessible (Sub-Topics + Objectives)
- [ ] Mastery levels and deck filenames populated for studied topics

## Trigger

**When:** User asks "what should I study next?", "next topic", "how am I doing?", or "what's my coverage?"
**Frequency:** On-demand — typically during study planning sessions

---

## Procedure

### Step 1: Request a recommendation (Human)

Say any of the following:
- "What should I study next?"
- "Next topic"
- "What's my coverage?"
- "How am I doing?"

> Tip: "What's my coverage?" triggers the summary stats mode instead of a single recommendation.

### Step 2: Query live data from Notion (Automation)

Pull all sub-topics and objectives from the two Notion databases. Never rely on cached or assumed state — always query live.

### Step 3: Apply priority logic and recommend (Automation)

Evaluate sub-topics against five priority tiers (highest first):

1. **Weak + no deck** — Known weakness with no study material created yet
2. **Weak + has deck, no comparisons** — Material exists but no cross-topic reinforcement
3. **Not Rated** — Unknown territory that needs assessment
4. **Moderate + no comparisons** — Good candidate for a comparison deck to deepen understanding
5. **Strong / Mastered** — Skipped unless user explicitly asks for review candidates

Within each tier, prefer sub-topics with more entries in "Maps To Objectives" (higher exam ROI).

### Step 4: Present recommendation (Automation)

Output a structured recommendation:
- Recommended topic name and parent objective
- Domain and current mastery level
- Why this one (1-2 sentences connecting to priority logic)
- Suggested study mode (A for new material, B for comparison if a natural partner exists)

**Decision Point:** If suggesting Mode B, name 1-2 comparison partners and explain why they pair well.

### Step 5: Coverage summary mode (Automation)

If triggered by a coverage question instead of a topic request:
- Count of sub-topics per mastery level
- Percentage with decks created
- Domains with the most Weak/Not Rated items
- If all sub-topics are Strong or Mastered, suggest a full practice exam

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Topic recommendation with reasoning | Claude response | Structured text |
| Coverage summary (if requested) | Claude response | Stats with domain breakdown |

## Quality Checks

- [ ] Recommendation pulls from live Notion data, not cached state
- [ ] Priority tier logic applied correctly — Weak + no deck always surfaces first
- [ ] Cross-objective ROI considered as a tiebreaker within tiers
- [ ] Mode B suggestions include named comparison partners with rationale

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Notion databases unreachable | Report the connection error. Suggest checking MCP configuration. |
| All sub-topics are Strong or Mastered | Congratulate the user and recommend a full practice exam. |
| Priority tiers produce no candidates | Check whether mastery levels are populated. Empty levels default to "Not Rated." |
| User disagrees with recommendation | Present the next 2-3 options from the priority queue with their reasoning. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `suggest-next-topic`
**GitHub Repository:** bengio777/agent-skills (synced via syncing-skills-to-github)

**Notion Data Sources:**
1. Sub-Topics DB: `collection://3583aa0d-82e1-45c0-bd9d-2b34abe9639a`
2. Objectives DB: `collection://8a24e2c2-ddbe-4369-ad7b-275c81181b20`

**Human Checkpoints:** Step 1 only (initiation)
**Workflow Type:** Automated (fully automated after trigger)
