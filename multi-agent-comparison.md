# Multi-Agent Project Comparison

A compare-and-contrast analysis of 6 multi-agent projects built with Claude, covering orchestration patterns, agent design, human-in-the-loop strategy, building blocks, and course alignment for the Hands-on AI Builders course.

---

## Summary Table

| Dimension | Forget-Me-Not | Universal Task Capture | Build Journal | MEDDPICC Discovery | Blog Post Skill | BG: Media Post-Coaching |
|-----------|--------------|----------------------|---------------|-------------------|----------------|------------------------|
| **Orchestration** | Cross-platform distributed | Routing & classification | Parallel quad-write | Parallel research + sequential | 4-agent sequential pipeline | Sequential handoff + approval gates |
| **Agent Count** | 4 (iOS Shortcut, Apps Script, Google Sheets, Claude Skill) | 4 (Capture, Classify, Route, Link) | 5 (1 skill + 4 write destinations) | 6 (1 orchestrator + 5 research threads) | 5 (1 orchestrator + 4 sub-skills) | 5 (same as Blog Post + coaching gates) |
| **Human-in-the-Loop** | Steps 1-3, 5 (initiation, input, metadata, confirmation) | Step 1 only (input); rest is automated | Steps 1, 4, 5 (activation, tier confirm, interview) | Steps 2-6 have review gates; Step 4 is fully human | Steps 1, 3, 6, 7, 9 (media, angle, draft review, editorial, reflection) | Same as Blog Post with coaching emphasis |
| **Building Blocks** | Skill, MCP (Notion, Sheets), iOS Shortcut, Apps Script | Skill, MCP (Notion x9 databases) | Skill, Hook, Templates, MCP (Notion, Sheets) | Prompt, Context x4, Agent (designed), MCP (future) | Skill x5 (orchestrator + 4 sub-skills) | Same skills + SOP documentation |
| **Platform Coverage** | Claude Code, iOS, Google Workspace | Claude Code, Notion | Claude Code, Notion, Google Sheets, Git | Claude Project | Claude Code | Claude Code |
| **Autonomy Level** | Automated | Automated | Augmented | Augmented | Augmented | Augmented |
| **Status** | In Production | Deployed | Deployed | Deployed | In Development | SOP Written |
| **Course Week** | Week 2 (Deterministic) | Week 2 (Deterministic) | Week 3 (Subagents + Agent Teams) | Week 3 (Subagents + Agent Teams) | Week 3 (Subagents + Agent Teams) | Week 3 (Subagents + Agent Teams) |

---

## Detailed Analysis

### 1. Orchestration Patterns

The 6 projects span 4 distinct orchestration patterns, each solving a different coordination problem:

**Cross-Platform Distributed (Forget-Me-Not):** Agents are physically separated across platforms (iOS, Google Workspace, Claude Code) and coordinate via HTTP webhooks. The iOS Shortcut POSTs to a Google Apps Script endpoint, which writes to both Notion and Google Sheets. The Claude Skill provides a parallel desktop path. No single orchestrator controls all agents — they coordinate through shared data stores.

**Routing & Classification (Universal Task Capture):** A single entry point fans out to 1 of 9 topic-specific databases based on AI classification. The pattern is: capture → classify → route → link. Each step is a logical agent within a single skill, but the routing logic creates a tree structure that scales with new categories.

**Parallel Write (Build Journal):** One orchestrator gathers data and generates a document, then writes it to 4 destinations concurrently. If any destination fails, the others still succeed. This is a fan-out pattern with graceful degradation — the first multi-destination write pattern in the portfolio.

**Parallel Research + Sequential Synthesis (MEDDPICC Discovery):** A master orchestrator dispatches 5 research threads that can run concurrently (LinkedIn, team mapping, regulatory, financial, company intel). Results are confidence-flagged and synthesized into a single briefing doc. The rest of the workflow is sequential with human gates.

**Sequential Pipeline (Blog Post Skill / Media Post-Coaching):** 4 specialized sub-skills execute in strict sequence: Media Analysis → Story Coaching → Draft & Multimedia → Editorial Coaching. Each agent produces output that becomes input for the next. The human reviews and approves between stages.

### 2. Agent Specialization

Projects differ in how they divide cognitive labor:

- **Forget-Me-Not** divides by *platform capability* — each agent handles what its platform does best (iOS for mobile capture, Apps Script for API orchestration, Claude for conversational intake)
- **Universal Task Capture** divides by *pipeline stage* — each logical agent handles one step in a fixed sequence
- **Build Journal** divides by *output destination* — the core logic is centralized, but writing is distributed across 4 backends
- **MEDDPICC Discovery** divides by *research domain* — each thread has specialized search strategies and source types
- **Blog Post Skill** divides by *creative discipline* — media analysis, narrative structure, drafting, and editing are distinct skills that mirror how a human editorial team works

### 3. Human-in-the-Loop Design

The projects fall on a spectrum from nearly autonomous to heavily gated:

**Most Autonomous:**
- **Universal Task Capture** — Human provides input; everything else is automated. Classification is AI-driven with a single clarifying question if ambiguous between two categories.
- **Forget-Me-Not** — Human provides question + metadata; both write destinations are fully automated.

**Moderate Gates:**
- **Build Journal** — Human activates, confirms tier selection, and answers interview questions. Generation and quad-write are autonomous.

**Heavily Gated:**
- **MEDDPICC Discovery** — 5 of 6 steps have human review gates. Step 4 (the actual call) is entirely human. Every AI output is reviewed before proceeding.
- **Blog Post Skill / Media Post-Coaching** — 5 of 9 steps involve human decisions. The coaching model means the human isn't just approving — they're being taught the craft at each stage.

**Key insight:** The autonomy level correlates with stakes. Low-stakes capture workflows (task logging, question logging) run with minimal human involvement. High-stakes workflows (deal qualification, content publishing) require heavy human gating. This is a deliberate design choice, not a limitation.

### 4. Building Blocks Taxonomy

| Block Type | Forget-Me-Not | Task Capture | Build Journal | MEDDPICC | Blog Post | Media Post-Coaching |
|------------|:---:|:---:|:---:|:---:|:---:|:---:|
| Skill | 1 | 1 | 1 | — | 5 | 5 |
| Prompt | — | — | — | 1 | — | — |
| Context MD | — | — | — | 4 | — | — |
| MCP | 2 (Notion, Sheets) | 1 (Notion x9) | 2 (Notion, Sheets) | — (future) | — | — |
| Hook | — | — | 1 | — | — | — |
| Template | — | — | 3 | — | — | — |
| iOS Shortcut | 1 | — | — | — | — | — |
| Apps Script | 1 | — | — | — | — | — |
| SOP | 1 | — | 1 | 1 | — | 1 |

**Observations:**
- Blog Post Skill is the only project using *sub-skills* (skills calling skills) — the most compositional pattern
- MEDDPICC is the only project using *Context MDs* as structured knowledge — the most context-heavy pattern
- Build Journal is the only project using *hooks* and *templates* — the most plugin-native pattern
- Forget-Me-Not is the only project spanning *non-Claude platforms* — the most architecturally distributed

### 5. Platform Coverage

| Platform | Projects Using It |
|----------|------------------|
| Claude Code | All 6 |
| Notion (MCP) | Forget-Me-Not, Universal Task Capture, Build Journal |
| Google Sheets (MCP) | Forget-Me-Not, Build Journal |
| iOS Shortcuts | Forget-Me-Not |
| Google Apps Script | Forget-Me-Not |
| Claude Project | MEDDPICC Discovery |
| Git/GitHub | Build Journal |

Forget-Me-Not is the outlier — it's the only project that coordinates agents across 3 different platforms (Claude Code, iOS, Google Workspace). The rest are Claude-centric with MCP connections to external data stores.

### 6. Autonomy Spectrum Mapping

Mapped to the course's Autonomy Spectrum (Deterministic → Collaborative → Autonomous):

| Level | Projects | Characteristics |
|-------|----------|----------------|
| **Deterministic Automated** | Forget-Me-Not, Universal Task Capture | Fixed rules, predictable output, minimal human judgment during execution |
| **Augmented / Collaborative** | Build Journal, MEDDPICC Discovery, Blog Post Skill, BG: Media Post-Coaching | AI drafts/generates, human reviews/steers, iterative feedback loops |

No project is fully autonomous (no human involvement). This reflects the course's teaching that autonomy is earned through trust and iteration — you start augmented and graduate to autonomous as the system proves reliable.

### 7. Maturity Assessment

| Status | Projects | What It Means |
|--------|----------|--------------|
| **In Production** | Forget-Me-Not | In daily active use, stable, documented, SOP written |
| **Deployed** | Universal Task Capture, Build Journal, MEDDPICC Discovery | Functional, registered, documented, but may not be in daily use |
| **In Development** | Blog Post Skill | Skill files designed, not yet pushed to GitHub |
| **SOP Written** | BG: Media Post-Coaching | Workflow documented, but backing skill still In Development |

The maturity gradient tracks a clear learning progression: the earliest builds (Forget-Me-Not) are the most mature because they've been through the most iteration. Later builds (Blog Post Skill) are more sophisticated in design but less battle-tested.

### 8. Course Week Alignment

**Week 2 — Deterministic + Collaborative Workflows:**
- **Forget-Me-Not** demonstrates a deterministic automated workflow with fixed classification and dual-write
- **Universal Task Capture** demonstrates deterministic routing with AI-powered classification

**Week 3 — Subagents + Agent Teams:**
- **Build Journal** demonstrates parallel write orchestration with graceful degradation
- **MEDDPICC Discovery** demonstrates parallel research orchestration with confidence-flagged synthesis
- **Blog Post Skill** demonstrates sequential multi-agent pipeline with specialized sub-skills
- **BG: Media Post-Coaching** demonstrates the same pipeline wrapped in a coaching workflow with human approval gates

---

## Key Insights

1. **Orchestration pattern diversity is the strongest signal of multi-agent maturity.** Building the same pattern (e.g., sequential pipeline) multiple times shows depth; building across patterns (routing, parallel, distributed, sequential) shows breadth. This portfolio demonstrates 4 distinct patterns.

2. **Human-in-the-loop is a design choice, not a limitation.** The portfolio shows deliberate escalation from automated capture workflows to heavily-gated creative and sales workflows. The gating intensity matches the stakes.

3. **Sub-skills are the most compositional pattern.** Blog Post Skill's orchestrator + 4 sub-skills mirrors how human teams specialize. This is the closest to a true "agent team" in the course's vocabulary.

4. **Cross-platform coordination is the hardest pattern.** Forget-Me-Not required solving webhook integration, iOS Shortcuts debugging, and Apps Script API design — fundamentally different challenges than Claude-internal orchestration.

5. **Context MDs are underutilized.** Only MEDDPICC uses structured knowledge files. The other projects could benefit from extracting reusable context (classification rules, template conventions, editorial standards) into standalone Context MDs.

6. **The progression tells a story.** From Forget-Me-Not (first build, cross-platform, production-ready) through MEDDPICC Discovery (most recent, most complex design, parallel research orchestration) — each project builds on lessons from the previous one. Build Journal was born from Forget-Me-Not's debugging pain. The Blog Post Skill is the most agent-rich design.
