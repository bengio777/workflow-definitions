# Agent Selection Coaching — Project Spec

**Project:** Agent Selection Coaching

---

## Capabilities Demonstrated

### 1. Skill-Powered Prompt Architecture
**Met.** Single SKILL.md drives a five-stage coaching flow with no external tool dependencies. Complexity Ladder framework, Signal-to-Pattern Map, and pattern catalog are all embedded as reference context.
**Evidence:** `~/.claude/skills/agent-selection-coach/SKILL.md`

### 2. Framework-Driven Architecture Decision Making
**Met.** Complexity Ladder (L0-L3) provides a structured climb from simplest to most complex, ensuring users don't jump to agents when a single prompt suffices. Signal-to-Pattern Map narrows to specific patterns.
**Evidence:** `~/.claude/skills/agent-selection-coach/references/decision-matrix.md`

### 3. Active Pushback on Over-Engineering
**Met.** At L0/L1 matches, the skill actively challenges the user to stay simple using the Escalation Checklist. This is the core differentiator — most users default to more complexity than needed.
**Evidence:** `~/.claude/skills/agent-selection-coach/SKILL.md`, `~/.claude/skills/agent-selection-coach/references/decision-matrix.md`

### 4. Pattern Catalog with Trade-offs
**Met.** Eight architecture patterns documented with descriptions, trade-offs, real examples, and named pitfalls. Trade-offs are personalized to the user's specific task during coaching.
**Evidence:** `~/.claude/skills/agent-selection-coach/references/pattern-catalog.md`

### 5. Structured Recommendation Delivery
**Met.** Final output includes a Decision Path (showing all branches checked and rejected) and a Structured Summary (pattern, rationale, trade-offs, pitfalls, next steps, Prompt Coach handoff).
**Evidence:** `~/.claude/skills/agent-selection-coach/SKILL.md`

### 6. Cross-Skill Handoff
**Met.** Recommendation delivery includes explicit handoff to Prompt Coach for the implementation phase, creating a natural workflow between the two coaching skills.
**Evidence:** `~/.claude/skills/agent-selection-coach/SKILL.md`

---

## Deliverables

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Skill definition | `~/.claude/skills/agent-selection-coach/SKILL.md` | Complete |
| 2 | Decision matrix | `~/.claude/skills/agent-selection-coach/references/decision-matrix.md` | Complete |
| 3 | Pattern catalog | `~/.claude/skills/agent-selection-coach/references/pattern-catalog.md` | Complete |
| 4 | Workflow definition | `~/Projects/workflow-definitions/agent-selection-coaching.md` | Complete |

---

## Review Criteria

| Criterion | Status | Evidence |
|-----------|--------|----------|
| Complexity Ladder climbed from L0 — no levels skipped | Pass | SKILL.md enforces bottom-up diagnostic questions |
| Simplicity challenge delivered at L0/L1 | Pass | Escalation Checklist referenced in decision-matrix.md |
| Signal-to-Pattern Map narrows to specific pattern | Pass | 5 signal-to-pattern mappings in decision-matrix.md |
| Trade-offs personalized to user's task | Pass | 4 consideration dimensions (latency, cost, complexity, error risk) |
| Anti-patterns cited by name | Pass | Anti-patterns documented in pattern-catalog.md |
| Decision Path shows checked and rejected branches | Pass | SKILL.md specifies two-part output format |
| Prompt Coach handoff included | Pass | Step 5 ends with explicit handoff |
| No external tool dependencies | Pass | Pure conversational skill |

---

## File Inventory

| File | Location | Purpose |
|------|----------|---------|
| SKILL.md | `~/.claude/skills/agent-selection-coach/SKILL.md` | Skill definition with Complexity Ladder coaching framework |
| decision-matrix.md | `~/.claude/skills/agent-selection-coach/references/decision-matrix.md` | Complexity Ladder, Signal-to-Pattern Map, Escalation Checklist |
| pattern-catalog.md | `~/.claude/skills/agent-selection-coach/references/pattern-catalog.md` | 8 architecture patterns with trade-offs, examples, pitfalls |
| agent-selection-coaching.md | `~/Projects/workflow-definitions/agent-selection-coaching.md` | Workflow definition / SOP |
| agent-selection-coaching-building-block-spec.md | `~/Projects/workflow-definitions/agent-selection-coaching-building-block-spec.md` | Building block specification |
| agent-selection-coaching-spec.md | `~/Projects/workflow-definitions/agent-selection-coaching-spec.md` | This file — project spec |
