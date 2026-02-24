# Prompt Coaching

**Status:** Deployed
**Type:** Augmented
**Business Process:** AI Capability Development
**Trigger:** User wants to write, improve, or get coaching on a prompt
**Process Outcome:** A production-ready prompt built through structured coaching, with the user understanding why each decision was made

**Assets Used:**
- prompt-coach (Claude Skill)
- references/technique-catalog.md (14 techniques across 3 tiers)
- references/domain-scaffolds.md (9 vocabulary scaffolds for creative/aesthetic domains)

---

## Overview

Interactive prompt engineering coaching that guides users through building best-in-class prompts via Socratic dialogue. Six-stage framework: Goal Extraction, Foundation Building, Technique Menu, Enrichment, Instruction Refinement, and Assembly & Challenge. Coaches the user to build the prompt themselves rather than writing it for them — building skill through vocabulary bridging, framework adherence, and challenge-based feedback.

## Prerequisites

- [ ] Claude Code with `prompt-coach` skill installed
- [ ] A task, idea, or draft prompt the user wants to work on

## Trigger

**When:** User wants to write a prompt, improve an existing prompt, or learn prompt engineering techniques
**Frequency:** On-demand — runs whenever a prompting need arises

---

## Procedure

### Step 1: Activate prompt coaching (Human)

Say any of the following:
- "Help me with this prompt"
- "I need a prompt for..."
- "Coach me through this"
- "How should I prompt this?"
- "Make this prompt better"
- Paste a draft prompt asking for feedback
- Describe a task you want AI to do (even without mentioning prompts)

### Step 2: Goal Extraction (AI + Human)

Ask: "What's the end goal here — what are you trying to achieve, and what does success look like?"

Even if the user provided a draft prompt, ask this. The goal behind the prompt often reveals gaps the prompt itself doesn't show.

**Decision Point:** Assess complexity to calibrate depth:

| Complexity | Signals | Approach |
|------------|---------|----------|
| Simple | One clear task, standard output | Quick pass through Stages 2-3, skip to 6 |
| Moderate | Custom format, domain context, multi-step | Stages 1-4, light touch on 5-6 |
| Complex | Multi-step workflow, specialized domain, high stakes | Full six stages |
| System/Skill | Persistent instructions, reusable across conversations | Full six stages with extra rigor |

### Step 3: Foundation Building (AI + Human)

Walk through six structural elements, one question per turn:

1. **Role** — Who should the AI be?
2. **Task** — What specific action should the AI take?
3. **Context** — What does the AI need to know? Audience, industry, constraints.
4. **Format / Output** — What should the response look like?
5. **Constraints** — What are the boundaries? What should it NOT do?
6. **Tone** — What's the communication register?

**Vocabulary Gap Detection:** When the user uses vibe language ("clean," "modern," "edgy," "powerful"), switch to vocabulary bridging mode. Use the appropriate domain scaffold from `references/domain-scaffolds.md` to decompose the vibe into named, tunable dimensions.

**Challenge:** Once all six elements are captured, review the foundation as a whole. Flag contradictions, misalignments, or stronger approaches the user hasn't considered.

### Step 4: Technique Selection (AI + Human)

Present a curated menu of 4-6 applicable techniques (from the 14-technique catalog) with a one-line rationale for each.

When the user picks a technique, challenge the selection. Ask *why* they chose it. Reinforce good reasoning or push back with a Socratic question if a better option exists.

**Anti-Patterns to Name:** The Vibe Fix, The Lazy Launch, The Kitchen Sink, The Phantom Audience, The Empty Role, The Contradiction, The Happy Path.

### Step 5: Enrichment (AI + Human)

Probe for layers the user might not have considered. Select the 1-3 most impactful for this case:

- Real-world constraints (budget, timeline, team size)
- Perspective shifts ("Argue from the CFO's perspective")
- Few-shot examples
- Edge cases and failure modes
- Context artifacts (reference docs, style guides)

### Step 6: Instruction Refinement (AI + Human)

Coach through delivery optimization:

- Directness — imperative vs. conversational framing
- Emotional stakes — urgency that improves thoroughness (used sparingly)
- Reasoning requests — "Think step by step" (only when warranted)
- Self-verification — model checks its own work (high-stakes outputs only)
- Metaphorical anchoring — evocative metaphors for creative tasks

### Step 7: Assembly & Challenge (AI)

Assemble the complete prompt in a clean code block. Then challenge it:

1. Internal consistency check — contradictions between elements?
2. Complexity calibration — is this appropriately sized?
3. Identify the weakest point — "If this prompt fails, it's probably because of..."
4. Testing guidance — "When you run this, look for..."

If the user wants to iterate, loop back to the relevant stage.

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Production-ready prompt | Claude response (code block) | Text |
| Coaching narrative | Claude conversation | Interactive dialogue |
| Technique recommendations | Claude response (table) | Markdown table |

## Quality Checks

- [ ] Goal is clearly defined before any prompt structure is discussed
- [ ] All six foundation elements addressed (or consciously skipped for simple prompts)
- [ ] Technique selection is justified, not arbitrary
- [ ] Vibe language decomposed into specific, tunable dimensions where detected
- [ ] Final prompt challenged for internal consistency and complexity calibration
- [ ] Complexity gauge matches actual task — over-engineering flagged as a coaching failure

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| User skips to "just write it for me" | Redirect: coaching builds skill, not dependency. Offer to demonstrate one element, then hand back control. |
| Vibe language persists after bridging | Try a different scaffold or use metaphorical anchoring as an alternative to parameter decomposition. |
| User picks a technique that doesn't fit | Challenge with a Socratic question. If they insist after hearing the trade-off, proceed — autonomy matters. |
| Prompt is over-engineered for a simple task | Name the anti-pattern ("The Kitchen Sink") and suggest stripping back to the minimum viable prompt. |
| User wants to iterate after assembly | Loop back to the relevant stage — don't restart from scratch. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `prompt-coach`
**GitHub Repository:** bengio777/agent-skills (synced via syncing-skills-to-github)

**Reference Files:**
1. `references/technique-catalog.md` — 14 techniques in 3 tiers (Foundational, Quality, Specialized)
2. `references/domain-scaffolds.md` — 9 vocabulary scaffolds (Visual Design, Cinematic, Presentation, Spatial, Writing Voice, Sonic, Brand Identity, Storytelling Structure, Metaphorical Prompting)

**Human Checkpoints:** Steps 1-6 (activation, goal, foundation, technique selection, enrichment, refinement)
**Workflow Type:** Augmented (AI coaches, human makes all prompt decisions)
