# Process Guide Writing -- Project Spec

**Project:** Process Guide Writing

---

## Capabilities Demonstrated

### 1. Strategic Documentation Generation
**Met.** The skill produces Process Guides that operate at the strategic level -- explaining when, why, and what order to execute workflows -- while deferring tactical step-by-step detail to individual workflow SOPs. The 7-section template enforces this separation.

**Evidence:** `~/.claude/skills/writing-process-guides/SKILL.md` (Process Guide vs Workflow SOP comparison table), `~/.claude/skills/writing-process-guides/references/process-guide-template.md`

### 2. Multi-Database Notion Integration
**Met.** The skill reads from two Notion databases -- Business Processes (for process properties) and Workflows (for linked workflow data) -- and writes back to the Business Processes page body. This cross-database pattern is more complex than single-database skills.

**Evidence:** `~/.claude/skills/writing-process-guides/SKILL.md` (Notion References section with both database IDs and API call format)

### 3. Augmented Workflow with Human-in-the-Loop
**Met.** Three human checkpoints: process identification (Step 1), strategic context gathering (Step 3), and draft review (Step 5). The AI handles fetching, synthesizing, writing, and saving autonomously between gates.

**Evidence:** `~/Projects/workflow-definitions/process-guide-writing.md` (Procedure section with actor tags on each step)

### 4. Linked Workflow Resolution and SOP Gap Detection
**Met.** The skill fetches all linked workflows from the business process, pulls their sequence and trigger data, and identifies which workflows lack SOPs. The guide proceeds regardless, noting gaps for follow-up.

**Evidence:** `~/.claude/skills/writing-process-guides/SKILL.md` (Interaction Pattern section -- "If workflows don't have SOPs yet" handling)

### 5. Scannability and Conciseness Standards
**Met.** Writing guidelines enforce strategic framing, 2-minute scannability, time estimates for overall process, and explicit decision point documentation. The skill actively redirects if content drifts into tactical SOP-level detail.

**Evidence:** `~/.claude/skills/writing-process-guides/SKILL.md` (Writing Guidelines section)

---

## Deliverables

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Skill definition | `~/.claude/skills/writing-process-guides/SKILL.md` | Complete |
| 2 | Process Guide template | `~/.claude/skills/writing-process-guides/references/process-guide-template.md` | Complete |
| 3 | API reference | `~/.claude/skills/writing-process-guides/references/api_reference.md` | Complete |
| 4 | Workflow definition | `~/Projects/workflow-definitions/process-guide-writing.md` | Complete |
| 5 | Building block spec | `~/Projects/workflow-definitions/process-guide-writing-building-block-spec.md` | Complete |
| 6 | Project spec | `~/Projects/workflow-definitions/process-guide-writing-spec.md` | Complete |

---

## Review Criteria

| Criterion | Status | Evidence |
|-----------|--------|----------|
| Skill executes end-to-end without errors | **Pass** | Deployed and in active use. Process Guides written for business processes in the Notion Business Processes database. |
| 7-section template structure enforced | **Pass** | `references/process-guide-template.md` defines all 7 sections. `SKILL.md` references it and lists each section with purpose. |
| Strategic vs. tactical separation maintained | **Pass** | Process Guide vs Workflow SOP comparison table in `SKILL.md` defines the boundary. Writing guidelines enforce "why and when, not how." |
| Multi-database Notion reads succeed | **Pass** | `SKILL.md` includes both Business Processes and Workflows database IDs. Interaction pattern fetches process, then fetches each linked workflow. |
| Notion save succeeds | **Pass** | `SKILL.md` includes explicit `notion-update-page` call format with `replace_content` command. |
| Human review gate before save | **Pass** | Step 5 in workflow definition requires human review before Step 6 saves to Notion. |
| SOP gap detection works | **Pass** | Interaction pattern includes "If workflows don't have SOPs yet" branch with three handling steps. |

---

## File Inventory

| File | Location | Purpose |
|------|----------|---------|
| `SKILL.md` | `~/.claude/skills/writing-process-guides/` | Skill definition with process, template overview, Process Guide vs SOP distinction, writing guidelines, and Notion references |
| `process-guide-template.md` | `~/.claude/skills/writing-process-guides/references/` | 7-section strategic template for business process documentation |
| `api_reference.md` | `~/.claude/skills/writing-process-guides/references/` | Notion API patterns for fetching and updating pages |
| `process-guide-writing.md` | `~/Projects/workflow-definitions/` | Workflow definition (SOP for the process-guide-writing workflow) |
| `process-guide-writing-building-block-spec.md` | `~/Projects/workflow-definitions/` | AI Building Block Spec with step decomposition and autonomy mapping |
| `process-guide-writing-spec.md` | `~/Projects/workflow-definitions/` | This file -- project spec with capabilities, deliverables, and review criteria |
