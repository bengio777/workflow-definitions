# Building Block Registration -- Project Spec

**Project:** Building Block Registration

---

## Capabilities Demonstrated

### 1. Multi-Type Asset Resolution
**Met.** The skill handles four distinct asset types (Skill, Agent, Prompt, Context MD) with type-specific metadata extraction paths. Resolution uses keywords, file path patterns, and user clarification as a fallback.

**Evidence:** `~/.claude/skills/registering-building-blocks/SKILL.md` (Asset Type Resolution table with source locations, identifier files, and name sources per type)

### 2. Automated Metadata Extraction
**Met.** The skill reads metadata directly from source files -- SKILL.md frontmatter for Skills, agent .md files for Agents -- rather than requiring manual input. This reduces friction and ensures accuracy.

**Evidence:** `~/.claude/skills/registering-building-blocks/SKILL.md` (Step 1: Extract Metadata section with per-type extraction rules)

### 3. Quick Start Prompt Generation
**Met.** The skill generates copy-paste-ready prompts that demonstrate each building block's primary use case. Guidelines enforce specificity, generality, and completeness. Examples provided for each asset type.

**Evidence:** `~/.claude/skills/registering-building-blocks/SKILL.md` (Step 2: Generate Quick Start Prompt section with guidelines and example table)

### 4. Duplicate Detection and Upsert Logic
**Met.** The skill searches for exact name matches before creating entries. Partial or similar names are explicitly excluded from duplicate detection. Found entries are updated; missing entries are created.

**Evidence:** `~/.claude/skills/registering-building-blocks/SKILL.md` (Step 3: Search for Existing Entry with "Critical" note on exact matching, Step 4: Create or Update with branching logic)

### 5. Batch Registration Support
**Met.** The skill processes multiple building blocks of mixed types in a single invocation, searching individually, batching creates and updates, and reporting totals by type.

**Evidence:** `~/.claude/skills/registering-building-blocks/SKILL.md` (Batch Registration section with 5-step process)

---

## Deliverables

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Skill definition | `~/.claude/skills/registering-building-blocks/SKILL.md` | Complete |
| 2 | Workflow definition | `~/Projects/workflow-definitions/building-block-registration.md` | Complete |
| 3 | Building block spec | `~/Projects/workflow-definitions/building-block-registration-building-block-spec.md` | Complete |
| 4 | Project spec | `~/Projects/workflow-definitions/building-block-registration-spec.md` | Complete |

---

## Review Criteria

| Criterion | Status | Evidence |
|-----------|--------|----------|
| Skill executes end-to-end without errors | **Pass** | Deployed and in active use. Building blocks registered across the AI Building Blocks database. |
| All four asset types supported | **Pass** | `SKILL.md` Asset Type Resolution table covers Skill, Agent, Prompt, and Context MD with distinct extraction paths. |
| Quick Start Prompts are actionable | **Pass** | Guidelines in `SKILL.md` enforce one-sentence, copy-paste-ready prompts. Examples provided for 5 building blocks across 3 asset types. |
| Duplicate detection prevents double entries | **Pass** | `SKILL.md` Step 3 searches by exact name match. Step 4 branches on match/no-match. "Critical" note explicitly excludes partial matches. |
| Batch registration works with mixed types | **Pass** | `SKILL.md` Batch Registration section defines 5-step process: search each, build two lists, generate prompts, update then create, report totals. |
| Notion schema matches database properties | **Pass** | `SKILL.md` Schema table lists all 6 properties (Name, Description, Asset Type, Platform, Quick Start Prompt, GitHub) with types matching the Notion database. |

---

## File Inventory

| File | Location | Purpose |
|------|----------|---------|
| `SKILL.md` | `~/.claude/skills/registering-building-blocks/` | Skill definition with asset type resolution, schema, extraction rules, Quick Start Prompt guidelines, search/create/update logic, and batch registration |
| `building-block-registration.md` | `~/Projects/workflow-definitions/` | Workflow definition (SOP for the registration workflow) |
| `building-block-registration-building-block-spec.md` | `~/Projects/workflow-definitions/` | AI Building Block Spec with step decomposition and autonomy mapping |
| `building-block-registration-spec.md` | `~/Projects/workflow-definitions/` | This file -- project spec with capabilities, deliverables, and review criteria |
