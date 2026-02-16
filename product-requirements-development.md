# Product Requirements Development

**Status:** Under Development
**Type:** Augmented
**Business Process:** Product Development
**Trigger:** New app idea or product concept
**Process Outcome:** Production-ready PRD

**Assets Used:**
- PRD Builder (Claude Skill)

---

## Overview

Generate production-ready Product Requirements Documents through structured discovery, version coaching, and iterative refinement. Every PRD bridges the gap between "I have an idea" and "this is ready to build" — structured so a solo founder, a dev team, or an AI coding agent can pick it up and start shipping immediately.

## Prerequisites

- [ ] Claude Code with `prd-builder` skill installed (includes 10 reference files)
- [ ] An app idea, product concept, or feature to scope
- [ ] Understanding of constraints: solo or team? Budget? Timeline? Tech preferences?

## Trigger

**When:** New app idea or product concept needs to be scoped and documented
**Frequency:** On-demand — runs for each new product or major version

---

## Procedure

### Step 1: Discovery interview (AI + Human)

Say "I have an app idea" or "help me write a PRD" to invoke the skill. Claude conducts a structured interview covering:

- **Product vision** — What problem? Who has it worst? What's the 2-3 year dream?
- **Jobs-to-be-done** — What 2-4 core jobs is the product hired to do?
- **Personas** — Day-one user in detail, then who else eventually (power users, admins, API consumers)
- **Feature brain dump** — Everything imagined, no filter. Claude actively deduplicates as the list grows.
- **Constraints** — Solo/team, budget, timeline, tech preferences
- **Competitive landscape** — What exists, what's the gap (web search used when helpful)
- **Monetization** — How and when this makes money
- **Kill criteria** — 2-3 measurable failure signals
- **Glossary seeding** — Domain terms defined as they emerge for consistency

> Tip: Adapt depth to the situation — a quick v0.1 sketch or a comprehensive deep dive are both valid.

### Step 2: Version coaching (AI + Human)

Claude evaluates each feature from the brain dump against six criteria:

1. **Persona dependency** — Does the target persona need this in v1?
2. **Dependency chain** — What must exist first?
3. **Complexity vs. value** — Does the build cost justify the value?
4. **Architectural implications** — Does a later version need a v1 design decision?
5. **Revenue unlock** — Does this enable monetization?
6. **Learning value** — Does this teach you something critical about users?

Claude presents a **Version Placement Map** (v1 → v2 → v3 → v4+) with feature IDs (F001, F002...) and rationale. Discuss disagreements — that's where the best decisions happen.

**Decision Point:** Claude offers optional deep-dive modules — competitive analysis, platform/integration, or revenue architecture. These are additive, never blocking: "Want to go deeper, or should I draft first and layer those in later?"

### Step 3: Generate the PRD (AI)

Claude generates the full PRD following a precise template structure:

- Problem statement, proposed solution, value proposition
- Complete feature set with user stories and acceptance criteria per P0/P1 feature
- Technical architecture with concrete stack and rationale
- Data model covering core entities and relationships
- Dependency graph and build order
- Version roadmap, success metrics, and kill criteria
- Decision log capturing key choices with rationale

Each feature carries a persistent **Feature ID** that cross-references across the entire document.

### Step 4: Validate and output (AI + Human)

Before presenting, Claude runs quality checks:

- Deduplication: no feature described twice under different names
- Cross-reference integrity: personas → features → jobs → versions all consistent
- Narrative coherence: problem → solution → architecture → roadmap tells a logical story
- Assumption risk ranking: 2x2 matrix (impact vs. validation cost)

Claude surfaces the **3 riskiest assumptions** baked into the PRD and asks you to gut-check them.

Output: Markdown file saved to working directory as `PRD-[ProductName]-v[X.X].md` with an implementation handoff section at the top.

### Step 5: Iterate and version (Human + AI)

For updates to an existing PRD:
- **Minor iterations** — Refine features, adjust scope, update based on feedback
- **Major version progressions** — Retrospective from prior version informs scope
- Feature IDs persist across all versions for traceability

### Step 6: Scaling checkpoints (AI + Human)

Evaluated at each major version boundary across three dimensions:
- **Technical scaling** — Can the architecture handle the next growth stage?
- **Product scaling** — Are new personas and jobs addressed?
- **Operational scaling** — Team, process, and infrastructure readiness

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Version Placement Map | Author review (in-session) | Markdown table with feature IDs |
| Production-ready PRD | Working directory | Markdown file (PRD-[Name]-v[X.X].md) |
| Risk assumptions summary | Author review (in-session) | Top 3 with gut-check prompt |
| Implementation handoff | Included in PRD header | Build chunking + dependency graph |

## Quality Checks

- [ ] Problem statement is specific and testable
- [ ] Every P0/P1 feature has user stories and acceptance criteria
- [ ] Feature IDs cross-reference consistently across all sections (feature set, roadmap, dependency graph, build order)
- [ ] Glossary defines all domain terms; terms used consistently throughout
- [ ] 3 riskiest assumptions surfaced and gut-checked with the author
- [ ] Narrative coherence: a stranger with zero context can understand the product

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Discovery interview goes too long | Adapt to a quick v0.1 — capture vision and constraints, draft lean, iterate later |
| Feature brain dump is overwhelming | Claude deduplicates actively during discovery. Consolidate overlapping capabilities and confirm canonical names. |
| v1 scope exceeds constraints | Don't pad the timeline — move features to v1.1 or v2. Cut aggressively for solo builders. |
| PRD sections contradict each other | Run the cross-reference integrity check before presenting. Persona → feature → job → version chains must all connect. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `prd-builder` with 10 reference files (template, examples, quality systems, version coaching, deep-dive modules, implementation handoff, iteration/scaling)

**GitHub Repository:** bengio777/prd-builder

**Optional Deep-Dive Modules:**
- Competitive Analysis — positioning, differentiators, table stakes
- Platform & Integration — per-version integration plan, API trajectory
- Revenue Architecture — tier structure, expansion revenue, pricing model

**Human Checkpoints:** Steps 1, 2, 4, 5, 6 (discovery input, version coaching decisions, assumption gut-check, iteration direction, scaling decisions)

**Workflow Type:** Augmented (human-driven, AI-structured)
