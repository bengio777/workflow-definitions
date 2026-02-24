# Workflow Naming — Project Spec

**Project:** Workflow Naming

---

## Capabilities Demonstrated

### 1. Skill-Powered Prompt with Notion Integration
**Met.** Single SKILL.md drives the naming and registration flow. Notion MCP used for two operations: reading Business Processes (Step 6) and writing the Workflow entry (Step 7).
**Evidence:** `~/.claude/skills/naming-workflows/SKILL.md`

### 2. Domain-Specific Naming Patterns
**Met.** Seven business domains (Sales, Marketing, Product, Education, Consulting, Operations, Finance) each have a distinct naming pattern that produces consistent, outcome-focused workflow names.
**Evidence:** `~/.claude/skills/naming-workflows/SKILL.md`, `~/.claude/skills/naming-workflows/references/examples-by-process.md`

### 3. Naming Convention Enforcement
**Met.** Four rules enforced on every generated name: 2-4 words, noun phrase, title case, self-explanatory without context. Five anti-patterns actively checked: verb phrases, too generic, too long, tool-focused, jargon.
**Evidence:** `~/.claude/skills/naming-workflows/SKILL.md`

### 4. Structured Description and Process Outcome Generation
**Met.** Descriptions follow a consistent pattern: `[Action verb] [object] [condition]. [Outcome].` Process outcomes are concrete deliverables (not "workflow completed").
**Evidence:** `~/.claude/skills/naming-workflows/SKILL.md`

### 5. Notion Database Write with Full Property Mapping
**Met.** Workflow entry created in Notion with all properties: Name, Description, Process Outcome, Business Process (relation), Sequence, Status, Type, Trigger, Apps, SOP Doc, Assets Used. Sequence auto-assigned as next multiple of 10.
**Evidence:** `~/.claude/skills/naming-workflows/SKILL.md`

### 6. Human-Gated Workflow with AI Acceleration
**Met.** AI generates options fast (Steps 2-4), human selects (Step 5), AI links and writes (Steps 6-8). The augmented pattern ensures names are user-approved before anything touches Notion.
**Evidence:** `~/Projects/workflow-definitions/workflow-naming.md`

---

## Deliverables

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Skill definition | `~/.claude/skills/naming-workflows/SKILL.md` | Complete |
| 2 | Examples by process | `~/.claude/skills/naming-workflows/references/examples-by-process.md` | Complete |
| 3 | Workflow definition | `~/Projects/workflow-definitions/workflow-naming.md` | Complete |

---

## Review Criteria

| Criterion | Status | Evidence |
|-----------|--------|----------|
| Names are 2-4 words, noun phrase, title case | Pass | Naming rules enforced in SKILL.md |
| Domain pattern applied correctly per business domain | Pass | 7 domain patterns with examples in SKILL.md |
| Anti-patterns checked (verb phrases, generic, long, tool-focused, jargon) | Pass | 5 anti-patterns documented in SKILL.md |
| Description follows action + outcome pattern | Pass | Template enforced: `[Action verb] [object] [condition]. [Outcome].` |
| Process outcome is tangible deliverable | Pass | SKILL.md rejects "completed" / "done" as outcomes |
| Sequence number uses multiples of 10, no conflicts | Pass | Queries existing workflows before assignment |
| Human confirms before Notion write | Pass | Step 5 gate between generation and registration |
| Business process linked via Notion relation | Pass | Step 6 queries Business Processes DB |
| Handles duplicate names | Pass | Search-first logic documented in failure modes |

---

## File Inventory

| File | Location | Purpose |
|------|----------|---------|
| SKILL.md | `~/.claude/skills/naming-workflows/SKILL.md` | Skill definition with naming conventions, domain patterns, and Notion schema |
| examples-by-process.md | `~/.claude/skills/naming-workflows/references/examples-by-process.md` | Real workflow name examples organized by business process |
| workflow-naming.md | `~/Projects/workflow-definitions/workflow-naming.md` | Workflow definition / SOP |
| workflow-naming-building-block-spec.md | `~/Projects/workflow-definitions/workflow-naming-building-block-spec.md` | Building block specification |
| workflow-naming-spec.md | `~/Projects/workflow-definitions/workflow-naming-spec.md` | This file — project spec |
