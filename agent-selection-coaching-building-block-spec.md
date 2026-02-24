# AI Building Block Spec: Agent Selection Coaching

## Execution Pattern
**Recommended Pattern:** Skill-Powered Prompt
**Reasoning:**
- Pure conversational coaching with no external tool calls
- Complexity Ladder framework and Signal-to-Pattern Map are reference data, not dynamic lookups
- Multi-turn Socratic dialogue — coaching flow, not orchestration
- No routing, parallelization, or chaining required
- Pattern catalog (8 patterns) and decision matrix loaded as skill context

---

## Scenario Summary
| Field | Value |
|-------|-------|
| **Workflow Name** | Agent Selection Coaching |
| **Description** | Guide users through selecting the right AI architecture complexity level — from single LLM call to autonomous agent — via the Complexity Ladder framework, with active pushback on over-engineering. |
| **Process Outcome** | Structured architecture recommendation with decision path and trade-offs |
| **Trigger** | User wants to build an agent, design an AI workflow, or select an architecture pattern |
| **Type** | Augmented |
| **Business Process** | AI Capability Development |

---

## Step-by-Step Decomposition
| Step | Name | Autonomy Level | Building Block(s) | Tools / Connectors | Skill Candidate | HITL Gate |
|------|------|----------------|--------------------|--------------------|-----------------|-----------|
| 1 | Task Discovery | AI + Human | LLM Generation, Information Extraction | — | `agent-selection-coach` | User describes task and success criteria |
| 2 | Complexity Check | AI + Human | LLM Generation, Classification (Ladder L0-L3) | references/decision-matrix.md | `agent-selection-coach` | User confirms complexity level after challenge |
| 3 | Signal Matching | AI + Human | LLM Generation, Pattern Matching | references/decision-matrix.md | `agent-selection-coach` | User confirms pattern selection |
| 4 | Trade-off Discussion | AI + Human | LLM Generation, Cost/Risk Analysis | references/pattern-catalog.md | `agent-selection-coach` | User reviews personalized trade-offs |
| 5 | Recommendation Delivery | AI-Semi-Autonomous | LLM Generation, Structured Output | — | `agent-selection-coach` | None (AI delivers decision path + summary) |

## Autonomy Spectrum Summary
```
|--AI+Human--------------------------------------AI-Semi-Auto--|
   Steps 1, 2, 3, 4                             Step 5
```

---

## Skill Candidates
### `agent-selection-coach`
**Purpose:** Coach users through architecture selection using the Complexity Ladder, pushing back on over-engineering and ensuring the user understands why a pattern fits.

**Inputs:**
| Input | Source | Required |
|-------|--------|----------|
| Task or workflow description | Human (conversation) | Yes |
| Success criteria | Human (Step 1) | Yes |
| Answers to diagnostic questions (scope, predictability, tool needs) | Human (Steps 2-3) | Yes |

**Outputs:**
| Output | Format | Destination |
|--------|--------|-------------|
| Decision path diagram | Markdown tree (checked/rejected branches) | Claude response |
| Architecture recommendation | Structured summary with pattern, rationale, trade-offs | Claude response |
| Trade-off analysis | Markdown table personalized to user's task | Claude response |
| Next steps with Prompt Coach handoff | Numbered list | Claude response |

**Decision Logic:**
- Complexity Ladder climbed from L0 upward — never skip levels
- At L0/L1 match, actively push back with Escalation Checklist before proceeding
- Signal-to-Pattern Map narrows from complexity level to specific pattern
- Trade-offs personalized to user's task (latency, cost, complexity, error risk)
- Anti-patterns cited by name when detected in user's plan

**Failure Modes:**
| Failure | Detection | Recovery |
|---------|-----------|----------|
| User insists on agent for a single-prompt task | User overrides simplicity challenge at L0/L1 | Challenge once with Escalation Checklist; if overridden, proceed and note override in recommendation |
| Task too vague to assess | Cannot determine scope, inputs, or outputs | Ask for concrete walkthrough: "Walk me through one run from start to finish" |
| Task straddles two patterns | Signals point equally to two patterns | Present both with the specific distinction; let user choose |
| User wants implementation, not architecture | User asks "how do I build this?" | Stop at recommendation + next steps; hand off to Prompt Coach |
| Trade-offs reveal overkill | Cost/complexity disproportionate to task value | Circle back to simpler option with concrete cost/complexity delta |

---

## Step Sequence and Dependencies
```
[1] Task Discovery
 └──► [2] Complexity Check (depends on: understood task + success criteria)
       └──► [3] Signal Matching (depends on: confirmed complexity level)
             └──► [4] Trade-off Discussion (depends on: matched pattern)
                   └──► [5] Recommendation Delivery (depends on: reviewed trade-offs)
                         └──► Handoff to Prompt Coach (optional)
```

**Dependency Map:**
- Steps are strictly sequential — each narrows the decision space
- Step 2 must start from L0 and climb up (no skipping)
- Step 3 references decision-matrix.md Signal-to-Pattern Map
- Step 4 references pattern-catalog.md for anti-patterns and trade-offs
- Step 5 produces two artifacts (Decision Path + Structured Summary) in a single output

---

## Prerequisites
- Claude Code installed and configured
- `agent-selection-coach` skill installed at `~/.claude/skills/agent-selection-coach/`
- Reference files present: `references/decision-matrix.md`, `references/pattern-catalog.md`

## Context Inventory
| Context | Source | When Loaded |
|---------|--------|-------------|
| Complexity Ladder (4 levels) | references/decision-matrix.md | Skill activation |
| Signal-to-Pattern Map | references/decision-matrix.md | Step 3 |
| Complexity Escalation Checklist | references/decision-matrix.md | Step 2 (L0/L1 challenge) |
| 8 architecture patterns with trade-offs | references/pattern-catalog.md | Steps 3-4 |
| Anti-pattern definitions | references/pattern-catalog.md | Step 4 |

## Tools and Connectors
| Tool | Purpose | Required |
|------|---------|----------|
| None | Pure conversational skill — no external tools | — |

## Recommended Implementation Order
1. Write SKILL.md with Complexity Ladder framework and coaching logic
2. Build decision-matrix.md (Ladder, Signal-to-Pattern Map, Escalation Checklist)
3. Build pattern-catalog.md (8 patterns with descriptions, trade-offs, examples, pitfalls)
4. Test with tasks at each Complexity Ladder level (L0 through L3)
5. Verify pushback behavior at L0/L1 and Prompt Coach handoff at Step 5

## Where to Run
- **Platform:** Claude Code
- **Execution:** Interactive conversation via `agent-selection-coach` skill
- **GitHub:** bengio777/agent-skills
- **Skill Location:** `~/.claude/skills/agent-selection-coach/`
