# Study Topic Suggestion — Project Spec

**Project:** Study Topic Suggestion

---

## Capabilities Demonstrated

### 1. Skill-Powered Prompt with External Data
**Met.** Single SKILL.md drives the entire flow. Notion MCP provides live data access for both recommendation and coverage modes, but all decision logic lives in the skill.
**Evidence:** `~/.claude/skills/suggest-next-topic/SKILL.md`

### 2. Deterministic Priority Algorithm
**Met.** Five-tier priority system applies fixed rules to live data: Weak+no deck > Weak+deck no comparisons > Not Rated > Moderate+no comparisons > Skip Strong/Mastered. No ambiguity in ranking.
**Evidence:** `~/.claude/skills/suggest-next-topic/SKILL.md`

### 3. Live Data Integration via Notion MCP
**Met.** Queries two Notion databases (Sub-Topics and Objectives) at runtime. Never relies on cached state — every recommendation reflects current progress.
**Evidence:** `~/.claude/skills/suggest-next-topic/SKILL.md`

### 4. Dual-Mode Operation
**Met.** Same skill handles two distinct use cases: single topic recommendation (trigger: "what should I study") and coverage summary (trigger: "how am I doing" / "coverage"). Mode detected from trigger phrase.
**Evidence:** `~/.claude/skills/suggest-next-topic/SKILL.md`

### 5. Exam ROI Tiebreaking
**Met.** Within priority tiers, sub-topics with more "Maps To Objectives" entries rank higher — maximizing exam coverage per study session.
**Evidence:** `~/.claude/skills/suggest-next-topic/SKILL.md`

### 6. Fully Automated After Trigger
**Met.** No human checkpoints after Step 1. Data retrieval, priority logic, and output formatting all run without intervention. This is the only fully automated workflow among the four coaching/suggestion skills.
**Evidence:** `~/Projects/workflow-definitions/study-topic-suggestion.md`

---

## Deliverables

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Skill definition | `~/.claude/skills/suggest-next-topic/SKILL.md` | Complete |
| 2 | Workflow definition | `~/Projects/workflow-definitions/study-topic-suggestion.md` | Complete |

---

## Review Criteria

| Criterion | Status | Evidence |
|-----------|--------|----------|
| Queries live Notion data, not cached state | Pass | SKILL.md enforces live query at every invocation |
| Priority tiers applied correctly (Weak+no deck first) | Pass | 5-tier algorithm documented in SKILL.md |
| Tiebreaker uses "Maps To Objectives" count | Pass | ROI logic specified in SKILL.md |
| Mode B suggestions include named comparison partners | Pass | SKILL.md requires partner names and rationale |
| Coverage summary includes per-domain breakdown | Pass | Summary mode outputs counts per mastery level and domain |
| Handles all-mastered edge case | Pass | Congratulates user and recommends practice exam |
| Handles Notion MCP failure | Pass | Reports error and suggests configuration check |
| No human intervention after trigger | Pass | Steps 2-5 are fully automated |

---

## File Inventory

| File | Location | Purpose |
|------|----------|---------|
| SKILL.md | `~/.claude/skills/suggest-next-topic/SKILL.md` | Skill definition with priority algorithm and dual-mode logic |
| study-topic-suggestion.md | `~/Projects/workflow-definitions/study-topic-suggestion.md` | Workflow definition / SOP |
| study-topic-suggestion-building-block-spec.md | `~/Projects/workflow-definitions/study-topic-suggestion-building-block-spec.md` | Building block specification |
| study-topic-suggestion-spec.md | `~/Projects/workflow-definitions/study-topic-suggestion-spec.md` | This file — project spec |
