# BG : Media Post-Coaching

**Status:** Under Development
**Type:** Augmented
**Business Process:** Marketing
**Trigger:** Author has a piece of media and the impulse to create a post
**Process Outcome:** A published Substack post — concise, multimedia-native, story + lesson format

**Assets Used:**
- write-substack-post (Claude Skill — main orchestrator)
- media-analysis (Sub-skill)
- story-coaching (Sub-skill)
- draft-multimedia (Sub-skill)
- editorial-coaching (Sub-skill)

---

## Overview

Coach the author through transforming raw media (video, photo, audio) into a tight, engaging, multimedia-rich Substack post (~5 minute read) that blends storytelling with insight. The workflow teaches the craft at every step — the goal is to build the author's writing ability, not just produce a post.

## Prerequisites

- [ ] Claude Code with `write-substack-post` skill and all sub-skills installed
- [ ] Source media available (local files, YouTube links, Spotify links, or iCloud references)
- [ ] Substack account with editor access
- [ ] Familiarity with the Amuse-Bouche framework (Anchor → Essential Details → Dismount → Hook)

## Trigger

**When:** Author has a piece of media and the impulse to create a post
**Frequency:** On-demand — roughly weekly cadence

---

## Procedure

### Step 1: Select source media (Human + AI)

Tell Claude what media you're working with — drop a file, paste a YouTube/Spotify link, or describe what you have.

Two paths:
- **You already know** — Provide file(s) or link(s). Claude confirms selection and proceeds.
- **You're browsing** — Claude coaches: "Look for media that hits you in one of three ways: an emotional reaction, a story behind it, or strong visual quality."

Accepted media types: local photos/clips, YouTube links (auto-embed), Spotify links (auto-embed), iCloud references.

### Step 2: Analyze media and extract story angles (AI)

Claude uses the **Media Analysis** sub-skill to:
1. Process the media (transcribe audio, extract EXIF metadata, describe visuals)
2. Cross-reference against three lenses: emotional resonance, narrative potential, aesthetic/experiential
3. Present 2-3 story angles with one-sentence pitches

> Tip: If no angles resonate, Claude asks: "What's the story behind this? What would you tell a friend about this moment?"

### Step 3: Choose a story angle (Human)

Review the proposed angles. Pick one, modify one, or describe your own. Claude confirms before moving on.

**Decision Point:** If the media is too thin for a ~5 minute read, Claude will flag it and suggest pairing with other media from the same experience.

### Step 4: Coach the story structure (AI + Human)

Claude uses the **Story Coaching** sub-skill to walk through the Amuse-Bouche framework — one component at a time:

1. **Anchor** (built first) — The single memorable moment the reader carries with them
2. **Essential Details** (built second) — Only the setup needed to make the Anchor land
3. **Dismount** (built third) — How it ends (satisfying conclusion, call-to-action, or callback)
4. **Hook** (built last) — The opening that earns the reader's next 5 minutes

At every stage: Claude proposes, you react, you confirm before moving on. Maximum 4 setup points — Claude enforces brevity: "Does the Anchor still land if we remove this?"

### Step 5: Draft the post with integrated multimedia (AI)

Claude uses the **Draft & Multimedia** sub-skill to:
1. Map each piece of media to a story beat (Opener, Anchor support, Closer, or Rhythm break)
2. Present the media map for your approval
3. Generate the full draft (~1,200-1,500 words) with media placement markers
4. Apply quality checks: word count, voice authenticity, media balance (max 1 piece per 300-400 words), pacing

### Step 6: Review the draft (Human)

Read through and answer Claude's questions:
- What feels right?
- What feels off?
- Where does it not sound like you?
- Any media placements that don't work?

Claude revises based on feedback. Maximum 2 revision rounds in this step.

### Step 7: Editorial coaching (AI + Human)

Claude uses the **Editorial Coaching** sub-skill to:
1. **Brevity pass** — Flag redundancy, unnecessary setup, trailing endings, throat-clearing
2. **Hook check** — Is the opening earning the reader's next 5 minutes?
3. **Dismount check** — Does the ending land or trail off?
4. **Length enforcement** — Identify specific cuts if over 1,500 words; suggest where to add depth if under 1,200

Each suggestion comes with: Issue, Suggestion, Why (the writing principle), and Your Call (accept/reject/modify). Maximum 2 editing rounds.

> Tip: If you accept everything without pushback, Claude will prompt you to defend at least one choice — building craft means having opinions about your own writing.

### Step 8: Format for Substack (AI)

Claude prepares the post for manual publishing:
1. Clean Substack-ready content with headings, paragraphs, and media placement markers
2. Subtitle recommendation
3. Tag/category suggestions
4. `media-sources.md` listing all media with source type and embed method
5. Publishing checklist: copy content, upload local photos, paste embed links, add subtitle, set tags, preview, publish

### Step 9: Post-publish reflection (Human + AI)

After confirming the post is live, Claude asks:
1. "What felt easy?"
2. "Where did you get stuck?"
3. "What would you steal from this post for next time?"
4. "Confidence rating: 1-5?"

Claude logs the reflection. After post 3+, surfaces patterns from previous reflections. After post 5, offers voice calibration.

---

## Outputs

| Output | Destination | Format |
|--------|-------------|--------|
| Media analysis with story angles | Author review (in-session) | Markdown |
| Story structure (Hook, Details, Anchor, Dismount) | Author review (in-session) | Markdown |
| Full draft with media placements | Author review (in-session) | Markdown (~1,200-1,500 words) |
| Editorial suggestions with principles | Author review (in-session) | Markdown |
| Substack-ready post | Substack editor (manual publish) | Formatted content |
| media-sources.md | Post working directory | Markdown |
| Post-publish reflection | Logged for pattern tracking | Markdown |

## Quality Checks

- [ ] Final post is ~1,200-1,500 words (~5 minute read)
- [ ] Every piece of media serves a specific story beat — no decorative filler
- [ ] Hook earns the reader's next 5 minutes; dismount lands without trailing
- [ ] Post sounds like the author — authentic voice preserved, not polished into generic AI tone

## When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Media is too thin for a full post | Pair with other media from the same experience, or pivot to a shorter format |
| No story angles resonate | Ask: "What would you tell a friend about this moment?" Build the angle from the author's own words |
| Author accepts all editorial suggestions without pushback | Prompt them to defend at least one choice — the goal is craft development, not compliance |
| Draft sounds generic or AI-generated | Flag passages with [VOICE CHECK]. Ask the author to rewrite in their own words. Protect personality over polish. |

---

## Automation Notes

**Platform:** Claude Code
**Skill:** `write-substack-post` (orchestrates 4 sub-skills: media-analysis, story-coaching, draft-multimedia, editorial-coaching)

**Human Checkpoints:** Steps 1, 3, 6, 7, 9 (media selection, angle choice, draft review, editorial decisions, reflection)

**Voice Preservation Rules (non-negotiable):**
- Never rewrite the author's phrasing — only tighten
- Colloquial language, fragments, and informal tone are features, not bugs
- If a passage has personality but is too long, suggest trimming, not replacing

**Workflow Type:** Augmented (human-driven, AI-coached)
