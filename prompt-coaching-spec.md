# Prompt Coaching — Project Spec

**Project:** Prompt Coaching

---

## Capabilities Demonstrated

### 1. Skill-Powered Prompt Architecture
**Met.** Single SKILL.md drives a multi-stage coaching flow with no external tool dependencies. All logic, decision trees, and reference lookups are embedded in the skill definition.
**Evidence:** `~/.claude/skills/prompt-coach/SKILL.md`

### 2. Structured Coaching via Socratic Dialogue
**Met.** Six-stage framework (Goal Extraction, Foundation Building, Technique Selection, Enrichment, Instruction Refinement, Assembly & Challenge) guides users through prompt construction one question at a time, building understanding alongside the artifact.
**Evidence:** `~/.claude/skills/prompt-coach/SKILL.md`, `~/Projects/workflow-definitions/prompt-coaching.md`

### 3. Reference-Driven Decision Making
**Met.** Technique catalog (14 techniques, 3 tiers) and domain scaffolds (9 vocabularies) provide structured knowledge that the skill curates and presents contextually rather than dumping wholesale.
**Evidence:** `~/.claude/skills/prompt-coach/references/technique-catalog.md`, `~/.claude/skills/prompt-coach/references/domain-scaffolds.md`

### 4. Vocabulary Gap Detection and Bridging
**Met.** When users use vibe language ("clean," "modern," "edgy"), the skill detects the gap and decomposes it into named, tunable dimensions using the appropriate domain scaffold.
**Evidence:** `~/.claude/skills/prompt-coach/references/domain-scaffolds.md`

### 5. Complexity-Calibrated Depth
**Met.** Complexity Gauge (Simple/Moderate/Complex/System) assessed at Goal Extraction determines how many stages to traverse, preventing over-engineering of simple prompts.
**Evidence:** `~/.claude/skills/prompt-coach/SKILL.md`

### 6. Anti-Pattern Recognition
**Met.** Seven named anti-patterns (The Vibe Fix, The Lazy Launch, The Kitchen Sink, The Phantom Audience, The Empty Role, The Contradiction, The Happy Path) are detected and named during coaching.
**Evidence:** `~/.claude/skills/prompt-coach/SKILL.md`

---

## Deliverables

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Skill definition | `~/.claude/skills/prompt-coach/SKILL.md` | Complete |
| 2 | Technique catalog | `~/.claude/skills/prompt-coach/references/technique-catalog.md` | Complete |
| 3 | Domain scaffolds | `~/.claude/skills/prompt-coach/references/domain-scaffolds.md` | Complete |
| 4 | Workflow definition | `~/Projects/workflow-definitions/prompt-coaching.md` | Complete |

---

## Review Criteria

| Criterion | Status | Evidence |
|-----------|--------|----------|
| Skill activates on prompt-related requests | Pass | Trigger phrases documented in workflow definition Step 1 |
| Six foundation elements asked one at a time | Pass | SKILL.md enforces single-question-per-turn pattern |
| Technique menu curated to 4-6 of 14 | Pass | Technique catalog provides full set; SKILL.md filters by task signals |
| Vocabulary gap detection triggers scaffold | Pass | Domain scaffolds cover 9 creative/aesthetic domains |
| Complexity Gauge calibrates session depth | Pass | Four levels documented with approach for each |
| Anti-patterns named when detected | Pass | Seven anti-patterns defined in SKILL.md |
| Assembly includes internal consistency check | Pass | Step 7 includes 4 challenge dimensions |
| No external tool dependencies | Pass | Pure conversational skill |

---

## File Inventory

| File | Location | Purpose |
|------|----------|---------|
| SKILL.md | `~/.claude/skills/prompt-coach/SKILL.md` | Skill definition with full coaching framework |
| technique-catalog.md | `~/.claude/skills/prompt-coach/references/technique-catalog.md` | 14 techniques across 3 tiers (Foundational, Quality, Specialized) |
| domain-scaffolds.md | `~/.claude/skills/prompt-coach/references/domain-scaffolds.md` | 9 vocabulary scaffolds for creative/aesthetic domains |
| prompt-coaching.md | `~/Projects/workflow-definitions/prompt-coaching.md` | Workflow definition / SOP |
| prompt-coaching-building-block-spec.md | `~/Projects/workflow-definitions/prompt-coaching-building-block-spec.md` | Building block specification |
| prompt-coaching-spec.md | `~/Projects/workflow-definitions/prompt-coaching-spec.md` | This file — project spec |
