# Workflow SOP Writing -- Project Spec

**Project:** Workflow SOP Writing

---

## Capabilities Demonstrated

### 1. Template-Driven Content Generation
**Met.** The skill uses an 8-section SOP template (`references/sop-template.md`) with type-specific adaptations for Manual, Augmented, and Automated workflows. The template enforces consistent structure while allowing the AI to adapt tone, detail level, and actor tagging based on workflow type.

**Evidence:** `~/.claude/skills/writing-workflow-sops/SKILL.md` (Template Overview and Type Adaptations sections), `~/.claude/skills/writing-workflow-sops/references/sop-template.md`

### 2. Notion Integration for Data Retrieval and Storage
**Met.** The skill fetches workflow properties from the Notion Workflows database (Name, Description, Type, Trigger, Apps, Assets Used) and saves the finalized SOP back to the workflow page body using `notion-update-page` with `replace_content`.

**Evidence:** `~/.claude/skills/writing-workflow-sops/SKILL.md` (Notion References section with database ID and API call format)

### 3. Augmented Workflow with Human-in-the-Loop
**Met.** Three human checkpoints: workflow identification (Step 1), procedure detail gathering (Step 3), and draft review (Step 5). The AI handles fetching, writing, and saving autonomously between gates.

**Evidence:** `~/Projects/workflow-definitions/workflow-sop-writing.md` (Procedure section with actor tags on each step)

### 4. Writing Quality Standards
**Met.** Explicit writing guidelines enforce action verbs, one action per step, explicit decision points as branches, and tips reserved for non-obvious gotchas only.

**Evidence:** `~/.claude/skills/writing-workflow-sops/SKILL.md` (Writing Guidelines section)

---

## Deliverables

| # | Deliverable | File | Status |
|---|-------------|------|--------|
| 1 | Skill definition | `~/.claude/skills/writing-workflow-sops/SKILL.md` | Complete |
| 2 | SOP template | `~/.claude/skills/writing-workflow-sops/references/sop-template.md` | Complete |
| 3 | Workflow definition | `~/Projects/workflow-definitions/workflow-sop-writing.md` | Complete |
| 4 | Building block spec | `~/Projects/workflow-definitions/workflow-sop-writing-building-block-spec.md` | Complete |
| 5 | Project spec | `~/Projects/workflow-definitions/workflow-sop-writing-spec.md` | Complete |

---

## Review Criteria

| Criterion | Status | Evidence |
|-----------|--------|----------|
| Skill executes end-to-end without errors | **Pass** | Deployed and in active use. SOPs written for multiple workflows in the Notion Workflows database. |
| 8-section template structure enforced | **Pass** | `references/sop-template.md` defines all 8 sections. `SKILL.md` references it and lists each section with purpose. |
| Type adaptations produce distinct outputs | **Pass** | Manual, Augmented, and Automated adaptations specified in `SKILL.md` Type Adaptations section. Each type adjusts detail level, actor tagging, and focus areas. |
| Notion save succeeds | **Pass** | `SKILL.md` includes explicit `notion-update-page` call format with `replace_content` command. |
| Human review gate before save | **Pass** | Step 5 in workflow definition requires human review before Step 6 saves to Notion. |
| Writing guidelines followed | **Pass** | Guidelines codified in `SKILL.md`: action verbs, one action per step, explicit decision points, minimal tips. |

---

## File Inventory

| File | Location | Purpose |
|------|----------|---------|
| `SKILL.md` | `~/.claude/skills/writing-workflow-sops/` | Skill definition with process, template overview, type adaptations, writing guidelines, and Notion references |
| `sop-template.md` | `~/.claude/skills/writing-workflow-sops/references/` | 8-section SOP template with type-specific adaptation rules |
| `workflow-sop-writing.md` | `~/Projects/workflow-definitions/` | Workflow definition (SOP for the SOP-writing workflow) |
| `workflow-sop-writing-building-block-spec.md` | `~/Projects/workflow-definitions/` | AI Building Block Spec with step decomposition and autonomy mapping |
| `workflow-sop-writing-spec.md` | `~/Projects/workflow-definitions/` | This file -- project spec with capabilities, deliverables, and review criteria |
