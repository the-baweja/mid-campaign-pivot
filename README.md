# Mid-Campaign Pivot — The Orchestrator for When a Campaign Starts Slipping

**Run the complete mid-campaign breakdown-response workflow in one command. Composes 3 Baweja Media diagnostic skills: Creative Performance Audit → Ad Fatigue Detector → Creative Iteration Engine. Then routes each diagnosed ad to the right downstream fix (Hook Generator, Copy Writer, or Landing Page Audit).**

This is part of the v2 layer of the AI Skills for Media Buyers series — orchestrators that compose the primitive skills into complete workflows. Mid-Campaign Pivot is the "fixing the running thing" workflow. Its sibling, [Pre-Launch Stack](https://github.com/the-baweja/pre-launch-stack), is the "starting a new thing" workflow.

## Install

**Claude Desktop (Cowork):** download [`mid-campaign-pivot.skill`](https://github.com/the-baweja/mid-campaign-pivot/releases/latest) → Settings → Skills → drop it in.

**Claude Code:**

```
git clone https://github.com/the-baweja/mid-campaign-pivot.git ~/.claude/skills/mid-campaign-pivot
```

## Why this skill exists

When a Meta campaign breaks, most operators react with one move — kill ads, refresh creative, blame the algorithm. The composed diagnostic gives you a different angle: **the Audit names the structural mistake, the Fatigue Detector names whether the leak is template fatigue or message fatigue, and the Iteration Engine generates the iterations to ship — but only on ads where the upstream diagnoses say iteration is the right move.**

Sometimes the right move is a kill. Sometimes it's a sibling-campaign rotation. Sometimes it's a template swap. Running the 3 diagnostic skills in isolation produces 3 reports the user has to mentally cross-reference. Composing them produces one Pivot Plan with the cascade pre-resolved.

## The 3-step orchestration

1. **Creative Performance Audit** → Diagnosis Layer (UCJ Metric Stack verdict per ad)
2. **Ad Fatigue Detector** → Fatigue Layer (Template vs Message fatigue fork — supersedes the Audit's frequency-based fatigue verdict)
3. **Creative Iteration Engine** → Iteration Layer (10+ iterations per ad that passes the iteration gate)

The iteration gate (the cascade's unique value-add):

- Real Winner + not Fatigued → iterate to scale
- Fatiguing with **Template** fatigue → iterate to template-swap
- Fatiguing with **Message** fatigue → route to Ad Hook Generator (NOT iterate)
- Loser or Collapsed → no iteration; kill or rotate to graveyard

## Sub-skill dependencies (the orchestrator pattern)

Mid-Campaign Pivot detects which sub-skills are installed at runtime. Two tiers:

**Primary diagnostics (3):**
- Creative Performance Audit
- Ad Fatigue Detector
- Creative Iteration Engine

**Downstream fixes (3):**
- Ad Hook Generator (for hook problems)
- Ad Copy Writer (for offer/copy problems)
- Landing Page Audit (for LP congruency problems)

For each missing PRIMARY skill, the orchestrator asks: install or degraded mode? For each missing DOWNSTREAM skill, the pivot plan names the diagnosis and recommends the skill — the user can install + run later.

## What you get

A branded Baweja Media Pivot Plan DOCX containing:

- Executive Summary with the headline finding from the cascade + top 3 actions
- **The Cascade visualization** — how each ad flows from Audit verdict → Fatigue verdict → Iteration gate → downstream skill handoff. The orchestrator's signature output.
- Per-Ad Diagnostic Cards (top 10 ads) with the cascade resolved end-to-end
- **The 14-Day Fix Sequence:**
  - Day 1-2: Zero-cost moves (sibling rotation, kill the Collapsed ad, budget redirect)
  - Day 3-7: Wave 1 iterations launch
  - Day 8-14: Wave 2 — validate + scale
- Series handoffs per ad to the right downstream skill, with the exact brief for each
- Quality flags for any sub-skill that ran in degraded mode
- "What if I do nothing" consequence framing

Plus the 3 individual sub-skill DOCXs as source files.

## The Andromeda Rule — why the cascade matters

Per the Ad Fatigue Detector teaching: Meta treats same template + different copy as the **same asset**. So if an ad is fatiguing because of template fatigue, generating copy iterations is wasted production budget — Meta still routes it to the same exhausted audience pool. Only template swaps or sibling rotation work.

The cascade catches this. Without it, the Iteration Engine would generate copy iterations for any "fatigue" verdict. With it, the Fatigue Detector splits Template vs Message — and the Iteration Engine only generates copy iterations for Message-fatigue ads.

## Customize Your Branding

Edit `references/branding.md` with your own colors, typography, and document structure.

## Example

```
Run Mid-Campaign Pivot on my Meta ad account.
Symptom: account-wide CPA up 25% over last 14 days.
Window: last 30 days.
Filter: sales-objective campaigns only.
```

Output: Mid-Campaign Pivot detected all 3 primary skills installed + 2 of 3 downstream (missing: Landing Page Audit). Ran the cascade. The Performance Audit flagged 7 ads with various problems. The Fatigue Detector confirmed 4 Template-fatigue, 2 Message-fatigue, 1 false positive (was actually budget-starved). The Iteration gate routed: 4 to Iteration Engine for template swap, 2 to Ad Hook Generator for hook refresh, 1 to scale (the false-positive ad). The Pivot Plan's Day 1-2 zero-cost moves: sibling-rotate the 4 Template-fatigue ads (estimated 5-10 day extension on $X spend, zero production cost). Day 3-7: 12 template-swap iterations launch. Series handoff: install Landing Page Audit and re-run for the 1 ad that wouldn't fit any of the above buckets — likely a post-click problem.

## Who this is for

Operators whose Meta accounts have started slipping and want the full diagnostic + pivot plan done in one orchestrated run, not 3 separate audits to mentally cross-reference.

This is one of two orchestrators in the **AI Skills for Media Buyers — v2** layer by [Baweja Media](https://bawejamedia.com). The original 10 primitive skills sit at v1.0 — orchestrators compose them.

---

## Want Baweja Media to coach your in-house team through the orchestration pattern + the rest of the system?

[→ Book a strategy call](https://webinar.sannidhyabaweja.com/vsl-lp-ind)

---

Built by [Baweja Media](https://bawejamedia.com) · MIT License
