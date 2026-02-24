# Agent Selection Coaching

**Status:** Deployed
**Type:** Augmented
**Business Process:** AI Capability Development
**Trigger:** User wants to build an agent, design an AI workflow, or select an architecture pattern
**Process Outcome:** A structured architecture recommendation with decision path, trade-offs, and actionable next steps — with the user understanding why the pattern fits

**Assets Used:**
- agent-selection-coach (Claude Skill)
- references/decision-matrix.md (Complexity Ladder, Signal-to-Pattern Map, Escalation Checklist)
- references/pattern-catalog.md (8 patterns with trade-offs, examples, and pitfalls)

---

## Overview

Interactive agent architecture coaching that guides users through selecting the right complexity level — from single LLM call to autonomous agent — via the Complexity Ladder framework. Five-stage process: Task Discovery, Complexity Check, Signal Matching, Trade-off Discussion, and Recommendation Delivery. Pushes back on over-engineering (the most common mistake) and ensures the user understands the decision, not just the answer. Hands off to Prompt Coach for implementation.

## Prerequisites

- [ ] Claude Code with `agent-selection-coach` skill installed
- [ ] A task, workflow, or automation idea the user wants to design

## Trigger

**When:** User wants to build an agent, design an AI workflow, choose an architecture pattern, or is describing a task they want to automate with AI
**Frequency:** On-demand — runs whenever an architecture decision needs to be made

---

## Procedure

### Step 1: Task Discovery (AI + Human)

Ask two questions (one at a time):
1. "What are you building? Describe the task or workflow."
2. "What does success look like? How will you know it's working?"

**What to listen for:**
- Scope — single task or multi-step process?
- Complexity signals — predictable or open-ended steps?
- Input/output clarity — can the user articulate what goes in and out?
- Existing constraints — timeline, cost, team size, technical stack

**Decision Point:** If the user jumps straight to "I need an agent" without describing the task, slow them down. Architecture follows from the task, not the other way around.

### Step 2: Complexity Check (AI + Human)

Climb the Complexity Ladder from Level 0 upward. Never skip ahead.

| Level | Name | Description |
|-------|------|-------------|
| 0 | Single LLM Call | One well-crafted prompt handles the entire task |
| 1 | Augmented LLM | Single call with tools, retrieval, or memory |
| 2 | Workflow | Fixed or dynamic multi-step orchestration |
| 3 | Autonomous Agent | Open-ended, model decides steps at runtime |

Ask diagnostic questions starting from Level 0:
- "Could this be done with a single well-crafted prompt?"
- "Does it need tools, retrieval, or memory?"
- "Are the steps predictable and repeatable, or open-ended?"
- "Does the number of steps depend on what the model discovers?"

**Decision Point:** If the task fits Level 0 or Level 1, PUSH BACK actively. Make the case for simplicity. Cite the Complexity Escalation Checklist: "You should only move up when you've fully optimized at the current level." The user can override, but the challenge must happen first.

### Step 3: Signal Matching (AI + Human)

Narrow from a complexity level to a specific architecture pattern using targeted questions:

- "Are your subtasks independent, or does each depend on the one before it?"
- "Do different types of input need fundamentally different handling?"
- "Can you define clear evaluation criteria for the output?"
- "Do you need the model to decide what steps to take at runtime?"

Map answers to patterns using the Signal-to-Pattern Map from `references/decision-matrix.md`:
- Fixed sequence → Prompt Chaining
- Independent subtasks → Parallelization
- Input-dependent routing → Routing
- Quality-gated iteration → Evaluator-Optimizer
- Open-ended with dynamic steps → Autonomous Agent

**Decision Point:** If the user's answers point to a simpler pattern than expected, reinforce the simpler option. If the task straddles two patterns, present both with the specific distinction.

### Step 4: Trade-off Discussion (AI + Human)

Surface the real costs personalized to the user's task:

| Consideration | What to Assess |
|---------------|---------------|
| Latency | Real-time or batch? How does the pattern affect UX? |
| Cost | Token volume and API usage for this specific workflow |
| Complexity | Can the user build and maintain this with their team/skill level? |
| Error risk | What happens when something fails? Demo or production? |

Reference anti-patterns from `references/pattern-catalog.md` when the user's plan shows symptoms (e.g., tool overloading, premature orchestration).

**Decision Point:** If trade-offs reveal the pattern is overkill, circle back and advocate for the simpler option.

### Step 5: Recommendation Delivery (AI)

Output two structured artifacts in a single message:

**Part A: Decision Path** — Shows the full path through the decision tree. Every branch visible — checked and rejected branches shown, matching branch gets a checkmark.

**Part B: Structured Summary:**
- Pattern name and complexity level
- Why this fits (2-3 sentences connecting task signals to the pattern)
- Key trade-offs table (personalized to the user's case)
- What to watch for (1-2 anti-patterns or pitfalls)
- Next steps (3 concrete actions)
- Handoff: "Ready to craft prompts for this pattern? Use the Prompt Coach skill."

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Decision path diagram | Claude response | Markdown tree |
| Architecture recommendation | Claude response | Structured summary |
| Trade-off analysis | Claude response | Markdown table |
| Next steps | Claude response | Numbered list |

## Quality Checks

- [ ] Task is clearly understood before any framework is applied
- [ ] Complexity Ladder climbed from Level 0 — no levels skipped
- [ ] Simplicity challenge delivered for Level 0/1 matches (even if user overrides)
- [ ] Trade-offs personalized to the user's specific task, not generic
- [ ] Anti-patterns cited by name when detected
- [ ] Recommendation includes the Prompt Coach handoff

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| User insists on an agent when a single prompt suffices | Challenge once with the Escalation Checklist. If they override after hearing the case, proceed — autonomy matters. Note the override in the recommendation. |
| Task is too vague to assess | Ask for a concrete example: "Walk me through one run of this task from start to finish." |
| Task straddles two patterns | Present both with the specific distinction. Let the user choose, then adjust trade-offs accordingly. |
| User wants implementation, not architecture | Stop at recommendation + next steps. Hand off to Prompt Coach for the implementation phase. |
| Trade-offs reveal the pattern is overkill | Circle back to the simpler option. Present the cost/complexity delta to make the case concrete. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `agent-selection-coach`
**GitHub Repository:** bengio777/agent-skills (synced via syncing-skills-to-github)

**Reference Files:**
1. `references/decision-matrix.md` — Complexity Ladder (4 levels), Signal-to-Pattern Map, Complexity Escalation Checklist, Anti-Patterns
2. `references/pattern-catalog.md` — 8 architecture patterns with descriptions, trade-offs, examples, and pitfalls

**Coaching Handoff:** Final output references `prompt-coach` skill for the implementation phase
**Human Checkpoints:** Steps 1-4 (task description, complexity confirmation, signal matching, trade-off review)
**Workflow Type:** Augmented (AI coaches through the decision, human makes architecture choice)
