# Plan: Referee pipeline enhancements (adapted from clo-author)

**Status:** COMPLETED (2026-07-07) — Tier 1 built and committed locally on `main`, not pushed. Table-format default set to no-significance-stars per user edit. Functional end-to-end pending a real PDF in `papers/`.
**Date:** 2026-07-07
**Goal:** Fold four Tier-1 ideas from clo-author into our `/referee` pipeline, keeping it lean and preserving our design philosophy (grounded-not-generated; judgment over mechanical score-averaging).

## Scope (Tier 1 only)
1. **Journal calibration (optional)** — `--journal [X]` tunes reviewers to a target journal; **default (no flag) = generic top-5** behavior.
2. **"What would change my mind"** — every *major* concern must name the specific test/evidence that would resolve it.
3. **FATAL / ADDRESSABLE / TASTE** — classify every major concern; group actionable items into MUST / SHOULD / MAY.
4. **Paper-type awareness** — detect reduced-form / structural / theory+empirics / descriptive and apply type-specific review dimensions + mandatory sanity checks.

**Out of scope (dropped/deferred):** `--stress` (dropped — `adversarial-referee` covers it), editor desk-review, dispositions + pet peeves, `--r2` R&R mode. Numeric weighted scoring is NOT adopted — we keep 1–5 ratings + a judgment-based recommendation.

## Attribution / licensing
clo-author has no LICENSE file (all-rights-reserved). We adapt the *concepts* only and **re-author** all text (esp. `journal-profiles.md`) in our own words. Credit Hugo Sant'Anna's clo-author in the README Origin note and the journal-profiles header.

---

## Changes by file

### New
- **`.claude/references/journal-profiles.md`** — re-authored profiles for top-5 (AER, ECMA, JPE, QJE, REStud) + key field journals (AEJ:Applied, AEJ:Policy, JHR, JPubE, JLE, JDE, RESTAT, J.Econometrics, AER:Insights). Each: focus · bar · what domain-referee adjusts · what methods-referee adjusts · typical concerns · (AEA table-format note where relevant). No "referee pool" field (dispositions dropped). "Add your own journal" template at the end.

### Skill — `.claude/skills/referee/SKILL.md`
- Parse optional `--journal [X]` from `$ARGUMENTS`.
- **Phase 1 (ingest):** additionally classify **paper type** (reduced-form / structural / theory+empirics / descriptive) and record it in the paper brief. If `--journal` given, load the profile (or note "profile not on file → generic top-5 + journal name").
- **Phase 2/3:** pass journal context (profile text or "generic top-5") + paper type to every specialist and to `adversarial-referee`.
- **Phase 4 (synthesize):** classify each major concern **FATAL / ADDRESSABLE / TASTE**; produce a **MUST address / SHOULD address / MAY push back** summary. Report header states calibration + paper type.

### Agents
- **`methods-reviewer.md`** — biggest change: replace reduced-form-only lens with **four paper-type dimension sets** + **mandatory sanity checks** (sign/magnitude/dynamics; parameter plausibility & model fit; prediction sharpness; construct validity) run *before* the rating. Add journal-calibration block + "what would change my mind" on majors.
- **`contribution-reviewer.md`** — add journal-calibration block, paper-type note (contribution of a descriptive paper = the measure, etc.), "what would change my mind."
- **`literature-reviewer.md`** — add journal-calibration block (positioning against that journal's frontier), "what would change my mind."
- **`writing-reviewer.md`** — add journal-calibration block incl. **AEA table-format** rule (no significance stars for AEA journals), "what would change my mind."
- **`adversarial-referee.md`** — add **TASTE** alongside FATAL/ADDRESSABLE; align its "what would rebut it" to the standard "what would change my mind" phrasing.

### Rules
- **`referee-report-protocol.md`** — header adds *Calibrated to* + *Paper type*; major-concern block gains **What would change my mind** and a **FATAL/ADDRESSABLE/TASTE** tag; add a **MUST/SHOULD/MAY** summary section. Keep 1–5 ratings + judgment-based overall (unchanged philosophy).
- **`pdf-ingestion.md`** — add a short "classify the paper type" step and record it in the brief; note the optional journal input.
- `review-verification.md`, `quality-gates.md`, `orchestrator-protocol.md` — light touch only (mention paper-type + calibration are part of a complete report in the gate checklist).

### Templates & docs
- **`templates/referee-report.md`** — mirror the protocol changes (header, major-concern fields, MUST/SHOULD/MAY block).
- **`CLAUDE.md`** — document `--journal`, paper-type awareness, the `references/` dir; refresh the pipeline diagram and dimension list.
- **`README.md`** — document optional journal calibration + paper types; add clo-author credit in Origin.
- **`MEMORY.md`** — `[LEARN]` entries: journal calibration is opt-in (default top-5); paper-type-aware dimensions; "what would change my mind" required on majors; FATAL/ADDRESSABLE/TASTE → MUST/SHOULD/MAY.

---

## Verification
- Structural: valid frontmatter on all agents/skills; `--journal` parsing documented; grep that every referenced journal-profile field and rule cross-reference resolves; no orphaned references.
- Consistency: report template ⇄ referee-report-protocol.md ⇄ agent output blocks all agree on the major-concern fields and the MUST/SHOULD/MAY structure.
- Functional end-to-end still pending a real PDF in `papers/` (none present). Note this in the summary.

## Commit (local only, no push)
Logical commits on `main`: `feat: journal calibration + paper-type-aware reviews` and `feat: what-would-change-my-mind + FATAL/ADDRESSABLE/TASTE` (or one combined `feat`), plus a `docs` commit. Will not push without an explicit go-ahead.
