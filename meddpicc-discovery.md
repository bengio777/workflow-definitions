# MEDDPICC Discovery Prep, Call, and Follow-up

**Status:** Under Development
**Type:** Augmented
**Business Process:** Revenue Operations / Customer Onboarding / Course Development
**Trigger:** A discovery call is scheduled with a Director of Security or above at a target account
**Process Outcome:** A qualified (or disqualified) opportunity with MEDDPICC criteria assessed, CRM updated, and next steps defined

**Assets Used:**
- MEDDPICC Discovery Workflow Prompt
- MEDDPICC Scoring Criteria
- MEDDPICC Discovery Questions
- Sales Stage Definitions
- MEDDPICC Qualification Framework

---

## Overview

End-to-end workflow for an enterprise AE to research, prepare for, execute, and follow up on a discovery call with a senior stakeholder, using MEDDPICC to qualify the opportunity. Produces a scored qualification baseline that serves as the stage gate into Needs Analysis/Value Proposition.

## Prerequisites

- [ ] Discovery call scheduled with a Director of Security or above at a target account
- [ ] Stakeholder name, title, and company are known
- [ ] Claude Project set up with all four context files pre-loaded (see Automation Notes)
- [ ] LinkedIn access for stakeholder and team research
- [ ] Access to public financial filing sources (SEC EDGAR, investor relations pages)

## Trigger

**When:** A discovery call is confirmed on the calendar with a qualified stakeholder
**Frequency:** On-demand — runs for every new discovery call

---

## Procedure

### Step 1: Draft agenda and confirmation email (AI)

Provide meeting inputs to Claude: stakeholder name, title, company, LinkedIn URL, how the meeting was booked, meeting date/time, who is joining from your side, who is joining from their side, any prior context, and your company's domain.

Claude drafts a confirmation email with a proposed agenda that commands the process while inviting collaboration. The email introduces team members joining and stays under 150-200 words.

> Tip: The tone should be confident and collaborative, not presumptuous. "Here's what I'd like to cover — what would you add or adjust?"

**Decision Point:** Who joins from your side depends on whether a VAR is present, stakeholder seniority, and whether the meeting is 1:1 or multi-party. Default: AE + SE.

### Step 2: Review and send confirmation email (Human)

Review Claude's draft for tone, accuracy, and team member introductions. Adjust as needed and send to the prospect.

### Step 3: Research stakeholder and organization (AI)

Claude conducts deep research across five threads:

1. **Individual stakeholder** — LinkedIn profile, career history, recent posts, mutual connections, education
2. **Team mapping** — Direct reports, peers, manager; product/competitor exposure flags for each
3. **Legislative/regulatory drivers** — Compliance mandates relevant to their vertical, cited by name with deadlines
4. **Financial filing analysis** — 10-K, 8-K, investor statements scanned for domain priority signals
5. **Company and industry context** — Vertical, competitors, market position, guiding principles, third-party reference companies

Every finding is flagged: Verified (sourced), Inferred (reasonable conclusion), or Gap (unknown — probe on the call).

### Step 4: Review research outputs (Human)

Review Claude's research package for accuracy. Flag what is strategically significant. Identify which findings change the call strategy — especially prior product/competitor exposure on the team and filing language that names your domain.

### Step 5: Build the call briefing doc (AI)

Claude synthesizes the validated research into a glanceable briefing doc with five sections: Rapport Angles, Company & Industry, Stakeholder Map, Intelligence Plays, and MEDDPICC Targeting. Each section uses bold titles with highlighted sub-bullets for rapid visual parsing during the live call.

The MEDDPICC Targeting section maps gaps to specific questions from the Discovery Questions reference, using TED framework levels appropriate to what is known.

### Step 6: Review briefing doc and align with team (Human)

Review the briefing doc. Prioritize which talking points to emphasize. Share with your SE (and Director of Sales if joining). Confirm everyone is aligned on the account story and strategy before the call.

### Step 7: Execute the discovery call (Human)

Run the call using the briefing doc and MEDDPICC framework. Key principles:

- Open with rapport — the angle is a human judgment call
- Confirm the agenda and invite additions
- Pivot to qualification within ~5 minutes
- Use TED framework: Tell me → Explain to me → Describe to me
- Maintain 75/25 prospect-to-seller talk ratio
- Close with mutual commitments: "If I do X, will you do Y?"

After the call, provide Claude with: transcript or notes, tone of the interaction (formal/business casual/casual), commitments made, and anything to exclude from follow-up.

### Step 8: Generate post-call follow-up and mutual action plan (AI)

Claude generates a call recap email matching the specified tone and a mutual action plan listing reciprocal commitments with owners and deadlines. Sensitive information flagged by the AE is excluded.

### Step 9: Review and send follow-up (Human)

Review tone and content. Adjust what is included or excluded. Finalize mutual commitments. Send the follow-up email.

### Step 10: Score the opportunity against MEDDPICC (AI)

Claude maps call evidence to each MEDDPICC element using the Scoring Criteria reference. Each element receives a rating (Strong/Partial/Gap/Unknown) with supporting evidence and a recommended next action. Stage gate minimums are assessed: Identified Pain, Champion, and Economic Buyer must each be at least Partial to advance.

Includes: deal stage recommendation, top 3 gaps to close, and recommended next step with attendee suggestions.

### Step 11: Review scores and make qualification decision (Human)

Review the MEDDPICC scorecard. Adjust ratings based on judgment and context the model may have missed. Make the final qualify, deprioritize, or disqualify decision.

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Confirmation email with agenda | Prospect (via email) | Email |
| Research package (5 sections) | AE review | Markdown with confidence flags |
| Call briefing doc | AE + SE (shared) | Markdown — glanceable format |
| Follow-up email with recap | Prospect (via email) | Email |
| Mutual action plan | Prospect + AE | Markdown |
| MEDDPICC scorecard | AE (internal) | Markdown table + narrative |

## Quality Checks

- [ ] Briefing doc is scannable at a glance — each section takes under 10 seconds to parse
- [ ] Follow-up email excludes sensitive competitive intelligence or internal politics shared by prospect
- [ ] MEDDPICC scorecard includes direct quotes or cited evidence for every Strong rating
- [ ] Mutual action plan has specific deliverables and owners on both sides

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Research returns thin results (private company, minimal LinkedIn) | Flag gaps explicitly in the briefing doc. Prioritize those elements as call objectives. |
| Prospect goes silent after follow-up | Review mutual commitments — were they specific enough? Follow up referencing the specific reciprocal agreement. |
| MEDDPICC scores are mostly Gap after first call | Normal for a first discovery. Focus on the top 3 gaps for the next interaction. If Identified Pain is Gap, question whether this deal belongs in the pipeline. |
| AI scores too optimistically | Apply the Strong vs. Partial test: can you quote the prospect, or are you paraphrasing your assumption? If paraphrasing, downgrade to Partial. |

---

## Automation Notes

**Platform:** Claude Project
**Project Name:** MEDDPICC Discovery Workflow

**Context Files Pre-loaded:**
1. MEDDPICC_Qualification_Framework.md
2. MEDDPICC_Discovery_Questions.md
3. MEDDPICC_Scoring_Criteria.md
4. Sales_Stage_Definitions.md

**Human Checkpoints:** Steps 2, 4, 6, 7, 9, 11 (all review and decision points)
**Workflow Type:** Augmented (human-driven, AI-augmented)
