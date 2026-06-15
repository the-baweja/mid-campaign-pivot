---
name: mid-campaign-pivot
description: "Run the complete mid-campaign breakdown-response workflow in one command. Orchestrates three Baweja Media diagnostic skills in sequence: Creative Performance Audit → Ad Fatigue Detector → Creative Iteration Engine, then routes each diagnosed ad to the right downstream skill (Ad Hook Generator, Ad Copy Writer, or Landing Page Audit). Use when a previously-performing Meta campaign starts slipping — rising CPA, dropping CTR, fatigue signals, scale plateau — and the user needs an end-to-end pivot plan, not three disconnected audits. Trigger on: mid-campaign pivot, campaign breakdown, campaign is slipping, CPA rising, performance dropping, fatigue setting in, ads breaking, need to fix campaign, scale plateau, account audit, things are not working. If a sub-skill is not installed, the orchestrator offers to install OR proceed in degraded mode using available data. Outputs three branded Baweja Media DOCXs plus a unified Pivot Plan with the 14-day fix-this-week sequence."
---

# Mid-Campaign Pivot — the orchestrator for when a campaign starts slipping

You are the orchestrator. Your job is NOT to do the audit work yourself — three specialist diagnostic skills already do that. Your job is to compose them in the right order, carry the diagnoses from one into the next, route fix-prescriptions to the right downstream skill, and assemble the unified Pivot Plan that names the top 3 actions for the next 14 days.

## The doctrine this skill operates on

When a Meta campaign breaks, most operators react with one move — kill ads, refresh creative, blame the algorithm. The composed diagnostic gives you a different angle: the Audit names the structural mistake, the Fatigue Detector names whether the leak is template fatigue or message fatigue, and the Iteration Engine generates the specific iterations to ship — but only if the upstream diagnoses say iteration is the right move (sometimes it's a kill, sometimes it's a rotation).

The composability rule: **diagnoses cascade.** The Audit names which ad has a hook problem vs an offer problem vs a fatigue problem. The Fatigue Detector confirms whether the "fatigue problem" is template or message. The Iteration Engine generates iterations ONLY for the ads where the Audit + Fatigue Detector both say "this can be iterated" — and skips ads that should be killed or rotated to sibling campaigns.

Running these three in isolation produces three reports the user has to mentally cross-reference. Composing them produces one Pivot Plan with the cascade pre-resolved.

## Preflight Check — dependency detection

Before running, detect which sub-skills are available.

The 3 primary sub-skills this orchestrator composes:

1. **`creative-performance-audit`** — UCJ Metric Stack diagnosis per ad
2. **`ad-fatigue-detector`** — Template vs Message fatigue fork
3. **`creative-iteration-engine`** — generates 10+ iterations per validated winner

Plus 3 downstream skills it routes to:

4. **`ad-hook-generator`** — for ads diagnosed with hook problems
5. **`ad-copy-writer`** — for ads diagnosed with offer/copy problems
6. **`landing-page-audit`** — for ads diagnosed with LP congruency problems

**At the start of every run:**

```
Mid-Campaign Pivot runs 3 diagnostic skills then routes to downstream fixes. Please confirm:

PRIMARY (required for full pivot):
1. Creative Performance Audit — [installed / not installed]
2. Ad Fatigue Detector — [installed / not installed]
3. Creative Iteration Engine — [installed / not installed]

DOWNSTREAM (for executing the fixes):
4. Ad Hook Generator — [installed / not installed]
5. Ad Copy Writer — [installed / not installed]
6. Landing Page Audit — [installed / not installed]

Each missing PRIMARY = degraded diagnosis at that step.
Each missing DOWNSTREAM = recommendation only, no execution.

Proceed?
```

**For each missing PRIMARY sub-skill:**

| Option | When to choose |
|---|---|
| Install it now | Recommended for any of the 3 primary skills |
| Proceed in degraded mode | If they want a quick run with limited data |
| Skip this layer | If they only need a subset (e.g., just the audit, no fatigue layer) |

**For each missing DOWNSTREAM sub-skill:**
Continue without it — the pivot plan still names the diagnosis and recommends the downstream skill. The user can install + run later.

## What you need from the user upfront

Before any sub-skill runs, gather the diagnostic context once.

1. **Meta ad account ID** — the account to audit (needed for MCP calls)
2. **The symptom** — what's happening that brought them here (rising CPA / dropping CTR / fatigue / scale plateau / something else)
3. **The time window** — when did it start? (last 7 days / last 30 days / longer)
4. **Sales-objective filter scope** — confirm we're filtering to sales-objective campaigns only (or specify a different filter)
5. **Constraints** — anything off-limits (don't touch certain ads, don't kill the legacy workhorse, etc.)

Print this back as a confirmation block. Locks in the **Pivot Brief**.

## The 3-step orchestration sequence

### Step 1 — Creative Performance Audit

**Sub-skill:** `creative-performance-audit`

**If installed:**
Invoke the sub-skill. Pass: Pivot Brief. The sub-skill will run its full UCJ Metric Stack diagnosis. Capture:
- Performance Tier Breakdown (Winners / Workhorses / Losers / Untested)
- Top 10 ads with per-ad verdicts (hook problem / offer problem / LP problem / fatigue / real winner / budget-starved winner)
- Stage-Balance Check verdict
- Routes per ad (which downstream skill fixes which ad)

This is the **Diagnosis Layer** — input for Steps 2 and 3.

**If NOT installed (degraded mode):**
Ask the user for a snapshot of their current ad-level metrics (CSV from Ads Manager, or top-10 by spend with key metrics). Apply the UCJ Metric Stack manually at lower resolution. Flag heavily: "Full audit gives per-ad verdicts on 73+ ads. Degraded mode covers top-10 only."

### Step 2 — Ad Fatigue Detector

**Sub-skill:** `ad-fatigue-detector`

**If installed:**
Invoke the sub-skill. Pass: Pivot Brief + Diagnosis Layer (specifically the ads flagged as "fatigue" by the Audit, plus any ads with declining trend signals).

The sub-skill will run 7-day rolling trend analysis. Capture:
- 5-stage classification per ad (Healthy / Watch / Fatiguing / Fatigued / Collapsed)
- Template vs Message fatigue fork per ad
- Sibling-campaign-rotation candidates (zero-cost fixes)
- Frugal Refresh candidates

This is the **Fatigue Layer** — input for Step 3.

**Critical:** the Fatigue Layer SUPERSEDES the Audit's fatigue verdicts where they conflict. The Audit uses snapshot frequency; the Fatigue Detector uses 7-day trends. Trust the trend.

**If NOT installed (degraded mode):**
Use the Audit's frequency-based fatigue verdicts as a proxy. Flag that template-vs-message fork is unavailable without the Detector — recommend the user install before iterating.

### Step 3 — Creative Iteration Engine

**Sub-skill:** `creative-iteration-engine`

**If installed:**
Invoke the sub-skill — but ONLY on ads that pass the iteration gate. The iteration gate (from composed Steps 1 + 2):
- Ad is a Real Winner (from Audit) AND not Fatigued/Collapsed (from Detector) → iterate to scale
- Ad is Fatiguing with TEMPLATE fatigue → iterate to template-swap
- Ad is Fatiguing with MESSAGE fatigue → route to Ad Hook Generator instead (not iterate)
- Ad is Loser or Collapsed → no iteration; kill or rotate to graveyard

Pass the gated ad list + the specific iteration intent (scale / template-swap / refresh) to the sub-skill. Capture:
- 10+ iterations per winning ad
- Wave roadmap (which to launch first)
- Iteration ceiling detection (when to stop iterating and start over)

This is the **Iteration Layer**.

**If NOT installed (degraded mode):**
Skip iteration generation. Output the iteration gate verdict per ad ("this ad should be iterated for template-swap — install the Iteration Engine to generate the variants").

## The Unified Pivot Plan — the final deliverable

After all 3 sub-skills have run (in either full or degraded mode), assemble a single **Pivot Plan DOCX**.

Structure (apply branding from `references/branding.md`):

1. **Cover page** — "[Brand Name] Mid-Campaign Pivot Plan" + date + sub-skill status
2. **Executive Summary** — the headline finding from the cascade (e.g., "Your 3 highest-spend ads are all in Template fatigue — sibling rotation is the zero-cost fix") + top 3 actions
3. **The Cascade** — a visualization showing how each ad flows from Audit verdict → Fatigue verdict → Iteration gate → downstream skill handoff. This is the orchestrator's unique value-add.
4. **Per-Ad Diagnostic Cards** — for each of the top 10 ads, one card showing the cascade resolved end-to-end:
   - Audit verdict (hook problem / fatigue / etc.)
   - Fatigue stage + type (if applicable)
   - Iteration gate result (iterate / route / kill)
   - Specific next action with the named downstream skill
5. **The 14-Day Fix Sequence**:
   - Day 1-2: Zero-cost moves (sibling rotation, kill the Collapsed ad, redirect budget). No production needed.
   - Day 3-7: Wave 1 iterations launch (template swaps for Template-fatigue ads, hook refreshes for Message-fatigue ads)
   - Day 8-14: Wave 2 — measure, validate, scale winners
6. **Series Handoffs** — explicit routes per ad to downstream skills with what to brief each one:
   - To Ad Hook Generator: "ads X, Y, Z need new hooks. Brief: locked angle = [...], format = [...]"
   - To Ad Copy Writer: "ad W needs offer rewrite. Brief: pain points from this audit = [...]"
   - To Landing Page Audit: "ad V is the LP congruency case. Brief: real CR vs target = [...]"
7. **The Source Files** — list of the 3 individual sub-skill DOCXs
8. **Quality flags** — degraded-mode flags + upgrade recommendations
9. **What if I do nothing** — explicit consequence framing: "If you don't act this week, expected account-wide CPA drift is +15-25% over 7 days based on the fatigue trend." (estimate from data)
10. **Work with us** — Baweja Media CTA

### Quality standards

- The Cascade visualization (section 3) is the orchestrator's signature output. Make it concrete: name specific ads moving through specific gates.
- Every ad in section 4 must have ONE next action. Not three options — the cascade has already resolved which fix applies.
- The 14-Day Fix Sequence (section 5) must distinguish zero-cost moves (do today) from production-required moves (need to brief). Zero-cost moves go FIRST.
- Sibling-Campaign Rotation must appear as the first move for any Template-fatigue ad — per the Andromeda Rule, this buys 5-10 days at zero production cost.
- Flag degraded sub-skills explicitly with a yellow warning per section so the user knows where to upgrade.

## What this orchestrator does NOT do

- It does NOT make the per-ad kill / iterate / rotate decision blindly. The cascade routes; the user confirms.
- It does NOT replace the depth of the individual sub-skill DOCXs. The Audit DOCX has 73-ad detail; this orchestrator summarizes top-10.
- It does NOT execute the fixes. Pre-Launch Stack composes for production; Mid-Campaign Pivot composes for diagnosis. The downstream skills (Hook Generator, Copy Writer, etc.) execute.

## Shared modules

- **Preflight Check + Dependency Detection** — same pattern as Pre-Launch Stack
- **Pivot Brief context format** — same shared-brief structure
- **Branding (`references/branding.md`)** — same Baweja Media output styling
- **Quality-flag mechanism** — same way of surfacing degraded-mode steps
- **The Cascade pattern** — applicable to any future orchestrator that composes diagnostic skills

## Series context

This is one of two orchestrator skills in the **AI Skills for Media Buyers — v2** layer. The other orchestrator is **Pre-Launch Stack** (composes the 5 production skills for new-campaign launch). Together they prove the skill-composition pattern: individual skills are primitives, orchestrators are workflows. Pre-Launch Stack is the "starting a new thing" workflow; Mid-Campaign Pivot is the "fixing the running thing" workflow.
