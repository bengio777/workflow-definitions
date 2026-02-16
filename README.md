# Workflow Definitions

Standardized operating procedures (SOPs) for AI-augmented and automated workflows.

Each file documents a complete workflow: overview, prerequisites, trigger, step-by-step procedure, outputs, quality checks, troubleshooting, and automation notes.

## Workflow Index

| Workflow | Type | Status | SOP |
|----------|------|--------|-----|
| [MEDDPICC Discovery Prep, Call, and Follow-up](meddpicc-discovery.md) | Augmented | Under Development | Complete |
| [Open Question Logging](open-question-logging.md) | Automated | In Production | Complete |
| [BG : Media Post-Coaching](bg-media-post-coaching.md) | Augmented | Under Development | Complete |
| [Product Requirements Development](product-requirements-development.md) | Augmented | Under Development | Complete |
| Weekly Blog Creation | Augmented | In Production | Icebox — no skill file, possible placeholder |
| Course Curriculum Design | Augmented | Under Development | Icebox — no skill file, asset "In Development" |
| New Customer Welcome | Automated | In Production | Icebox — no file on disk, may live in Claude Project |

## Structure

Each SOP follows a consistent template:

- **Header** — Status, Type, Business Process, Trigger, Process Outcome, Assets Used
- **Overview** — What the workflow accomplishes and why
- **Prerequisites** — What must exist before starting
- **Trigger** — When and how the workflow starts
- **Procedure** — Step-by-step with (Human), (AI), or (Automation) labels
- **Outputs** — Deliverables with destinations and formats
- **Quality Checks** — How to verify success
- **When Things Go Wrong** — Common problems and solutions
- **Automation Notes** — Platform, assets, human checkpoints

## Canonical Locations

SOPs live in two places for redundancy:

1. **This repo** — Central index of all workflow definitions
2. **Project repos** — Each workflow's SOP also lives as `WORKFLOW-DEFINITION.md` in its asset's GitHub repo

| Workflow | Project Repo |
|----------|-------------|
| MEDDPICC Discovery | [MEDDPICC_Discovery](https://github.com/bengio777/MEDDPICC_Discovery) |
| Open Question Logging | [Forget-Me-Not__OpenQuestionLogger](https://github.com/bengio777/Forget-Me-Not__OpenQuestionLogger) |
| Product Requirements Development | [prd-builder](https://github.com/bengio777/prd-builder) |
| BG : Media Post-Coaching | No project repo yet |
