# AI Building Block Spec: Study Topic Suggestion

## Execution Pattern
**Recommended Pattern:** Skill-Powered Prompt
**Reasoning:**
- Single skill with deterministic priority logic — no multi-step orchestration needed
- Notion MCP provides live data access within the skill execution
- Two modes (recommendation vs. coverage summary) handled by input classification, not routing
- Priority tiers are a fixed algorithm, not dynamic decision-making
- No human intervention after trigger — fully automated execution

---

## Scenario Summary
| Field | Value |
|-------|-------|
| **Workflow Name** | Study Topic Suggestion |
| **Description** | Query the Security+ SY0-701 progress tracker in Notion and recommend the next study topic using a five-tier priority system that factors mastery level, deck status, cross-topic reinforcement, and exam ROI. |
| **Process Outcome** | Prioritized study topic recommendation with reasoning |
| **Trigger** | User asks what to study next or requests coverage stats |
| **Type** | Automated |
| **Business Process** | Education |

---

## Step-by-Step Decomposition
| Step | Name | Autonomy Level | Building Block(s) | Tools / Connectors | Skill Candidate | HITL Gate |
|------|------|----------------|--------------------|--------------------|-----------------|-----------|
| 1 | Request recommendation | Human | — | — | — | User initiates with trigger phrase |
| 2 | Query live Notion data | Automation | Data Retrieval | Notion MCP | `suggest-next-topic` | None |
| 3 | Apply 5-tier priority logic | Automation | Classification, Sorting | — | `suggest-next-topic` | None |
| 4 | Present recommendation | Automation | LLM Generation (structured output) | — | `suggest-next-topic` | None |
| 5 | Coverage summary mode | Automation | Data Aggregation, LLM Generation | Notion MCP | `suggest-next-topic` | None |

## Autonomy Spectrum Summary
```
|--Human----Automated/Deterministic-------------------------------|
   Step 1   Steps 2, 3, 4, 5
```

---

## Skill Candidates
### `suggest-next-topic`
**Purpose:** Query Notion progress data and apply deterministic priority logic to recommend the highest-value next study topic, or summarize coverage stats.

**Inputs:**
| Input | Source | Required |
|-------|--------|----------|
| Trigger phrase (topic request or coverage request) | Human (conversation) | Yes |
| Sub-Topics database (live) | Notion MCP | Yes |
| Objectives database (live) | Notion MCP | Yes |

**Outputs:**
| Output | Format | Destination |
|--------|--------|-------------|
| Topic recommendation | Structured text (topic, domain, mastery, reasoning, study mode) | Claude response |
| Coverage summary | Stats with domain breakdown | Claude response |

**Decision Logic:**
- Mode detection: "what should I study" triggers recommendation; "coverage" / "how am I doing" triggers summary
- Five-tier priority (highest first):
  1. Weak + no deck
  2. Weak + has deck, no comparisons
  3. Not Rated
  4. Moderate + no comparisons
  5. Strong / Mastered (skipped unless explicitly requested)
- Tiebreaker within tiers: more "Maps To Objectives" entries = higher exam ROI
- Study mode suggestion: Mode A (new material) or Mode B (comparison deck with named partners)

**Failure Modes:**
| Failure | Detection | Recovery |
|---------|-----------|----------|
| Notion databases unreachable | MCP connection error | Report error; suggest checking MCP configuration |
| All sub-topics Strong or Mastered | No candidates in tiers 1-4 | Congratulate user; recommend full practice exam |
| Empty mastery levels | Sub-topics exist but no mastery data | Default to "Not Rated" — surfaces in tier 3 |
| User disagrees with recommendation | User pushes back on suggestion | Present next 2-3 options from priority queue with reasoning |

---

## Step Sequence and Dependencies
```
[1] Request recommendation (Human)
 └──► [2] Query live Notion data
       └──► [3] Apply 5-tier priority logic (depends on: live data)
             ├──► [4] Present recommendation (topic mode)
             └──► [5] Coverage summary (coverage mode)
```

**Dependency Map:**
- Step 2 must query live — never use cached or assumed state
- Step 3 depends on complete data from Step 2 (both Sub-Topics and Objectives databases)
- Steps 4 and 5 are mutually exclusive paths determined by trigger phrase at Step 1
- Mode B recommendations in Step 4 require cross-referencing sub-topics for comparison partners

---

## Prerequisites
- Claude Code installed and configured
- `suggest-next-topic` skill installed at `~/.claude/skills/suggest-next-topic/`
- Notion MCP configured with access to Security+ SY0-701 Progress Tracker
- Sub-Topics and Objectives databases populated with mastery levels and deck filenames

## Context Inventory
| Context | Source | When Loaded |
|---------|--------|-------------|
| Sub-Topics database (mastery, deck status, comparisons, objective mappings) | Notion MCP (live query) | Step 2 |
| Objectives database (exam domains, objective descriptions) | Notion MCP (live query) | Step 2 |
| 5-tier priority algorithm | SKILL.md | Skill activation |

## Tools and Connectors
| Tool | Purpose | Required |
|------|---------|----------|
| Notion MCP | Query Sub-Topics and Objectives databases | Yes |

## Recommended Implementation Order
1. Write SKILL.md with priority algorithm and mode detection logic
2. Verify Notion MCP connectivity to both databases
3. Test recommendation mode with known data (Weak + no deck topic exists)
4. Test coverage summary mode
5. Test edge cases: all mastered, empty mastery levels, MCP failure

## Where to Run
- **Platform:** Claude Code
- **Execution:** Interactive conversation via `suggest-next-topic` skill
- **GitHub:** bengio777/agent-skills
- **Skill Location:** `~/.claude/skills/suggest-next-topic/`
