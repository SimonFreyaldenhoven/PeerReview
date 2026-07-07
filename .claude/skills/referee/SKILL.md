---
name: referee
description: Produce a top-journal-quality economics referee report from a paper PDF. Orchestrates parallel specialist reviewers (methods, contribution, literature, writing), an adversarial toughest-referee pass, then synthesizes one structured report and fact-checks every claim against the paper. Use when the user supplies a paper to review or says "referee this", "review this paper", "write a referee report".
argument-hint: "[path to a PDF, or a filename in papers/]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Bash", "Task", "WebSearch", "WebFetch"]
---

# Referee — Multi-Agent Peer Review

Turn a paper PDF into the kind of report a conscientious referee at a top economics journal would write: specific, constructive, grounded in the actual text, and honest about what is fatal vs. fixable.

**Input:** `$ARGUMENTS` — a path to a `.pdf` (or `.tex`/`.qmd`), or a filename in `papers/`. If empty, list the PDFs in `papers/` and ask which one.

Follow the rules in `.claude/rules/`: `pdf-ingestion.md`, `referee-report-protocol.md`, `review-verification.md`, and `orchestrator-protocol.md`.

---

## Phase 1 — Locate & ingest

1. Resolve the paper: direct path → `papers/$ARGUMENTS` → glob for a partial match. If several match, ask.
2. Get length first: `pdfinfo` (or read the outline) so you know how to chunk. **Read the whole paper**, in ~5-page chunks for long PDFs. Do not skim.
3. Write a faithful **paper brief** to `referee_reports/.work/<name>_brief.md` capturing: title & authors; abstract; research question; data & sample; research design/estimator; identifying assumptions as stated; headline results (with the numbers); main tables/figures and what each shows; the reference list. The brief is orientation for the sub-agents — it is **not** a substitute for the source, and every reviewer is told to verify claims against the PDF itself.

## Phase 2 — Specialist reviews (parallel)

Launch **all four** specialists in a single message (concurrent Task calls). Pass each the **PDF path** and the **brief path**:

- `methods-reviewer` — identification + econometric specification
- `contribution-reviewer` — question, novelty, whether conclusions follow
- `literature-reviewer` — citation completeness & positioning (may use the web; flags web claims)
- `writing-reviewer` — clarity, notation, self-contained exhibits

Each returns the structured block defined in its agent file.

## Phase 3 — Adversarial pass

Launch `adversarial-referee` with the PDF, the brief, and the four specialist outputs. It hunts for reject-worthy objections and anything the specialists missed, labeling each objection `FATAL` or `ADDRESSABLE`.

## Phase 4 — Synthesize

You (the orchestrator) merge everything into one report — do not just concatenate:
- **Deduplicate & reconcile.** Fold overlapping points together. Where reviewers disagree, adjudicate and note the disagreement if it matters.
- **Rank.** Promote/demote between major and minor by *how much the conclusions depend on the point*, not by which reviewer raised it. FATAL adversarial objections lead the major concerns.
- **Recommend.** Choose an overall recommendation and set the 1–5 dimension ratings, informed by (not mechanically averaged from) the specialist ratings.
- Write the report in the **structured-critique format** from `referee-report-protocol.md`.

## Phase 5 — Fact-check (anti-fabrication)

Before saving, run the checklist in `review-verification.md` over your own draft: every location reference must be real, every "they didn't do X" must be something you actually confirmed is absent, no invented coefficients or citations. Web-sourced literature claims stay flagged `[web]`. Drop or soften anything you cannot stand behind.

## Phase 6 — Deliver

- Save to `referee_reports/referee_report_<sanitized-name>_<YYYY-MM-DD>.md` using `templates/referee-report.md`.
- Print the **Summary Assessment** and the recommendation to the user, with a one-line pointer to the saved file.
- Leave the brief in `referee_reports/.work/` (gitignored) for traceability.

---

## Principles

- **Grounded, not generated.** Every criticism cites an exact location; nothing is invented. A wrong objection destroys the report's credibility faster than a missed one.
- **Constructive.** Each concern carries a concrete suggestion or the analysis that would resolve it.
- **Calibrated.** Distinguish fatal flaws from things a good R&R fixes. Not everything is equally important.
- **Fair.** Acknowledge genuine strengths; good work deserves recognition.
- **The referee, not the editor.** Recommend, justify, and raise the hard questions — the report should let an editor make the call.
