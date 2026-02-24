# AI Building Block Spec: Prompt Coaching

## Execution Pattern
**Recommended Pattern:** Skill-Powered Prompt
**Reasoning:**
- Pure conversational coaching with no external tool calls
- All logic lives in a single SKILL.md with reference files
- Multi-turn dialogue driven by Socratic questioning, not orchestration
- No routing, parallelization, or chaining required — one continuous coaching flow
- Reference catalogs (14 techniques, 9 scaffolds) are loaded as skill context, not retrieved dynamically

---

## Scenario Summary
| Field | Value |
|-------|-------|
| **Workflow Name** | Prompt Coaching |
| **Description** | Guide users through building production-ready prompts via Socratic coaching across six stages: goal extraction, foundation building, technique selection, enrichment, instruction refinement, and assembly with challenge. |
| **Process Outcome** | Production-ready prompt with user understanding why each decision was made |
| **Trigger** | User wants to write, improve, or get coaching on a prompt |
| **Type** | Augmented |
| **Business Process** | AI Capability Development |

---

## Step-by-Step Decomposition
| Step | Name | Autonomy Level | Building Block(s) | Tools / Connectors | Skill Candidate | HITL Gate |
|------|------|----------------|--------------------|--------------------|-----------------|-----------|
| 1 | Activate | Human | — | — | — | User initiates coaching |
| 2 | Goal Extraction | AI + Human | LLM Generation, Complexity Assessment | — | `prompt-coach` | User confirms goal and complexity |
| 3 | Foundation Building | AI + Human | LLM Generation, Vocabulary Gap Detection | references/domain-scaffolds.md | `prompt-coach` | User answers each of 6 elements |
| 4 | Technique Selection | AI + Human | LLM Generation, Curated Filtering | references/technique-catalog.md | `prompt-coach` | User selects techniques; Socratic challenge |
| 5 | Enrichment | AI + Human | LLM Generation | — | `prompt-coach` | User confirms enrichment layers |
| 6 | Instruction Refinement | AI + Human | LLM Generation | — | `prompt-coach` | User confirms delivery optimizations |
| 7 | Assembly & Challenge | AI-Semi-Autonomous | LLM Generation, Self-Verification | — | `prompt-coach` | None (AI assembles and challenges) |

## Autonomy Spectrum Summary
```
|--Human----------AI+Human---------------------------AI-Semi-Auto--|
   Step 1         Steps 2, 3, 4, 5, 6               Step 7
```

---

## Skill Candidates
### `prompt-coach`
**Purpose:** Coach users through structured prompt engineering via Socratic dialogue, building skill rather than dependency.

**Inputs:**
| Input | Source | Required |
|-------|--------|----------|
| User's task description or draft prompt | Human (conversation) | Yes |
| Goal and success criteria | Human (Step 2) | Yes |
| Foundation element answers (role, task, context, format, constraints, tone) | Human (Step 3) | Yes |
| Technique selections | Human (Step 4) | Yes |

**Outputs:**
| Output | Format | Destination |
|--------|--------|-------------|
| Production-ready prompt | Markdown code block | Claude response |
| Coaching narrative | Interactive dialogue | Claude conversation |
| Technique recommendations | Markdown table | Claude response |

**Decision Logic:**
- Complexity Gauge determines coaching depth: Simple (skip to assembly), Moderate (light touch on refinement), Complex/System (full six stages)
- Vocabulary Gap Detection triggers domain scaffold lookup when vibe language detected
- Technique menu curated to 4-6 of 14 based on task characteristics
- Anti-pattern detection (7 named patterns) triggers coaching corrections

**Failure Modes:**
| Failure | Detection | Recovery |
|---------|-----------|----------|
| User skips to "just write it" | User requests AI-generated prompt without engagement | Redirect: coaching builds skill. Demonstrate one element, hand back control. |
| Vibe language persists | User repeats vague terms after bridging attempt | Try alternate scaffold or switch to metaphorical anchoring |
| Technique mismatch | User picks technique that doesn't fit task signals | Socratic challenge with rationale; proceed if user insists after hearing trade-off |
| Over-engineering | Simple task getting Complex treatment | Name "The Kitchen Sink" anti-pattern; strip to minimum viable prompt |
| Iteration loop | User unsatisfied after assembly | Loop to relevant stage, not full restart |

---

## Step Sequence and Dependencies
```
[1] Activate
 └──► [2] Goal Extraction (depends on: user input)
       └──► [3] Foundation Building (depends on: confirmed goal + complexity)
             └──► [4] Technique Selection (depends on: completed foundation)
                   └──► [5] Enrichment (depends on: selected techniques)
                         └──► [6] Instruction Refinement (depends on: enriched prompt)
                               └──► [7] Assembly & Challenge (depends on: all prior stages)
                                     └──► (optional) Loop back to any stage
```

**Dependency Map:**
- Steps are strictly sequential — each builds on the prior stage's output
- Step 3 requires all 6 foundation elements addressed before proceeding
- Step 4 references technique-catalog.md (loaded at skill activation)
- Step 3 may reference domain-scaffolds.md (triggered by vocabulary gap detection)
- Step 7 can loop back to any prior step for iteration

---

## Prerequisites
- Claude Code installed and configured
- `prompt-coach` skill installed at `~/.claude/skills/prompt-coach/`
- Reference files present: `references/technique-catalog.md`, `references/domain-scaffolds.md`

## Context Inventory
| Context | Source | When Loaded |
|---------|--------|-------------|
| 14-technique catalog (3 tiers) | references/technique-catalog.md | Skill activation |
| 9 domain vocabulary scaffolds | references/domain-scaffolds.md | On vocabulary gap detection |
| Complexity Gauge definitions | SKILL.md | Skill activation |
| 7 anti-pattern definitions | SKILL.md | Skill activation |

## Tools and Connectors
| Tool | Purpose | Required |
|------|---------|----------|
| None | Pure conversational skill — no external tools | — |

## Recommended Implementation Order
1. Write SKILL.md with full coaching framework and decision logic
2. Build technique-catalog.md with 14 techniques across 3 tiers
3. Build domain-scaffolds.md with 9 vocabulary scaffolds
4. Test end-to-end with a Simple, Moderate, and Complex prompt each
5. Verify anti-pattern detection and vocabulary gap bridging

## Where to Run
- **Platform:** Claude Code
- **Execution:** Interactive conversation via `prompt-coach` skill
- **GitHub:** bengio777/agent-skills
- **Skill Location:** `~/.claude/skills/prompt-coach/`
