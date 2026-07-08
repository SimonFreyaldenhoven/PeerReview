# Plan: Add `consistency-reviewer` specialist to /referee

**Status:** COMPLETED
**Date:** 2026-07-08

## Context
The panel (methods · contribution · literature · writing + adversarial) had no owner for **internal numeric consistency** — whether the *same quantity* agrees wherever it appears (appendix table vs. main-text figure, a number the text quotes off an exhibit, subtotals vs. total, %Δ / t-stat vs. the coefficients they derive from, unit/scale slips). writing-reviewer covers exhibit *interpretability*, not cross-exhibit numeric reconciliation. A table↔figure contradiction is a correctness gap that can be FATAL, and nothing systematically caught it.

## Decisions (with user)
1. **Standalone agent**, not folded into writing-reviewer (correctness lens ≠ presentation lens; matches the narrow-lens design).
2. **No new rating dimension.** Findings feed Major/Minor concerns (tagged to the existing dimension they threaten — usually Econometric Specification) **and** a new `## Consistency Check` report section. Mirrors the adversarial-referee (contributes concerns, scores no dimension). Ratings table stays 5 dimensions.
3. **Scope = cross-check + light re-derivation, read-only** (`Read, Grep, Glob`). Shows its arithmetic; does not re-judge econometric correctness (that stays methods-reviewer).

## Boundary
- consistency-reviewer — the paper's numbers agree *with each other* across text/tables/figures/appendix.
- writing-reviewer — each exhibit interpretable in isolation + abstract prose matches results.
- Phase-5 fact-check (`review-verification.md`) — *our report* quotes the paper correctly.

## Files changed
- **New:** `.claude/agents/consistency-reviewer.md` (house style; no rating line; `Consistency Check` audit block).
- `.claude/skills/referee/SKILL.md` — frontmatter; Phase 2 four→five + 5th bullet; Phase 3 hand-off; Phase 4 compile the Consistency Check + fold mismatches; Phase 5 both-location verification.
- `.claude/rules/referee-report-protocol.md` + `templates/referee-report.md` — `## Consistency Check` section after Minor Concerns; `Internal Consistency` added to MC Dimension enum; **ratings table unchanged**.
- `.claude/rules/quality-gates.md` — gate 5 (+consistency pass), gate 7 (four→five).
- `.claude/rules/orchestrator-protocol.md`, `.claude/agents/adversarial-referee.md`, `CLAUDE.md`, `README.md` — count/roster bumps (four→five specialists; README panel Five→Six agents). "five *dimensions*" refs left untouched.
- `.claude/rules/review-verification.md` — item 7: internal-consistency mismatches are two-sided (both locations, verify both).
- `MEMORY.md` — updated the multi-agent learning to 5 specialists + a new `[LEARN:review]` entry.

## Verification
- **Static (done):** no stale "four specialist" counts; agent wired into SKILL/orchestrator/adversarial/CLAUDE/README/protocol/template; ratings tables still exactly 5 dimensions + Overall in both protocol and template; "five dimensions" phrasing preserved.
- **Live (optional, not yet run):** `/referee papers/severen.pdf` — confirm a 5th parallel reviewer, a populated `## Consistency Check` section with two-location ISSUEs, no new ratings row.
