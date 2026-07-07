# Plan: Pivot repo to a peer-review (referee report) pipeline

**Status:** DRAFT — awaiting approval
**Date:** 2026-07-07
**Goal:** Transform this fork from an academic-slides workflow into a focused tool: *supply a PDF → get a high-quality economics/econometrics referee report.* Everything slide/Quarto/teaching-related is removed.

## Decisions (from requirements clarification)
- **Domain:** Economics / econometrics (identification, specification, robustness, contribution).
- **Architecture:** Multi-agent + adversarial — parallel specialist reviewers → adversarial "toughest referee" pass → orchestrator synthesizes one report → anti-fabrication claim check.
- **Git:** Rewrite `main` directly. Commit locally in logical steps. **No push to GitHub** without an explicit later go-ahead.
- **Report format:** Keep the current structured-critique layout (summary + recommendation, strengths, major/minor concerns, referee objections, 1–5 dimension ratings).

---

## Step 0 — Checkpoint (safety)
- There are 85 uncommitted working-tree changes. Commit them as-is first: `checkpoint: pending infra edits before peer-review pivot`. Nothing is lost; everything stays recoverable in git history.

## Step 1 — REMOVE (slide / research / teaching infrastructure)
**Skills (delete):** compile-latex, create-lecture, deploy, extract-tikz, qa-quarto, translate-to-quarto, visual-audit, slide-excellence, pedagogy-review, proofread, validate-bib, devils-advocate, deep-audit, interview-me, research-ideation, data-analysis, review-r. *(Old `review-paper` folded into the new `referee` skill, then removed.)*

**Agents (delete):** beamer-translator, quarto-critic, quarto-fixer, slide-auditor, tikz-reviewer, pedagogy-reviewer, r-reviewer, proofreader, verifier.

**Rules (delete):** beamer-quarto-sync, no-pause-beamer, tikz-visual-quality, single-source-of-truth, r-code-conventions, replication-protocol, proofreading-protocol, exploration-fast-track, exploration-folder-protocol, knowledge-base-template, orchestrator-research.

**Folders / root files (delete):** Slides/, Quarto/, Figures/, Preambles/, docs/, guide/, explorations/, scripts/ (slide utilities), Bibliography_base.bib.

## Step 2 — KEEP & ADAPT (reusable backbone)
- **Skills:** commit, context-status, learn, lit-review (repurposed for citation-completeness checks).
- **Agents:** domain-reviewer → basis for the new methods reviewer.
- **Rules:** plan-first-workflow, session-logging, quality-gates (reframed), meta-governance (updated), verification-protocol → `review-verification.md` (anti-fabrication), pdf-processing → `pdf-ingestion.md`, orchestrator-protocol (reframed to the referee pipeline).
- **Templates:** requirements-spec, session-log, quality-report (kept); add referee-report template.
- **Hooks:** keep all (context-monitor, log-reminder, notify, pre/post-compact, protect-files, verify-reminder); prune slide references from verify-reminder.py.
- **settings.json:** drop xelatex/bibtex/quarto/pdf2svg/Rscript/sync_to_docs permissions; keep git/gh/python3/ls/mkdir + PDF tools (pdfinfo, gs, pdftk).

## Step 3 — BUILD the referee pipeline
**New agents (`.claude/agents/`):**
1. `methods-reviewer` — identification strategy + econometric specification (SEs, functional form, sample selection, inference, estimator appropriateness).
2. `contribution-reviewer` — research question, novelty, framing, motivation, whether conclusions follow from evidence.
3. `literature-reviewer` — citation completeness, accurate characterization of prior work, missing/mis-cited references (may use web search / lit-review).
4. `writing-reviewer` — clarity, structure, notation consistency, self-contained tables/figures, abstract fidelity, typos.
5. `adversarial-referee` — the top-5-journal "reasons to reject": fatal objections, what the specialists missed.

Each specialist returns **structured findings** (strengths, major concerns, minor concerns, in-lane referee objections, a 1–5 rating) and **must cite exact PDF locations** (page/section/table/eq) — no fabrication.

**New skill `referee` (`/referee [path-or-papers/file.pdf]`)** — orchestrates:
1. Locate & read the PDF end-to-end (chunked for long papers); write a faithful structured **paper brief** to `referee_reports/.work/<name>_brief.md`.
2. Launch the 4 specialists **in parallel** (paper path + brief path).
3. Run `adversarial-referee` on the paper + specialist outputs (finds fatal flaws + gaps).
4. **Synthesize** one report: dedupe/reconcile across reviewers, rank major vs minor, resolve contradictions, assign final ratings + overall recommendation.
5. **Anti-fabrication pass:** verify every specific claim (location refs, "not clustered", "didn't cite X") against the paper; drop/soften anything unverifiable.
6. Save to `referee_reports/referee_report_<name>_<date>.md` using the template.

**New rules:** `referee-report-protocol.md` (structure, constructive tone, major/minor distinction, rating rubric), plus the adapted `review-verification.md` and `pdf-ingestion.md`.

**New folders:** `papers/` (drop PDFs here, with README) and `referee_reports/` (output).

## Step 4 — DOCS & CONFIG rewrite
- **CLAUDE.md:** new project identity, folder structure, commands, skill quick-ref, referee dimensions/rubric.
- **README.md:** self-contained "drop a PDF → get a referee report" quick start (no dependency on the removed guide site).
- **MEMORY.md:** prune slide/guide-specific `[LEARN]` entries; keep generic workflow/governance ones; add peer-review learnings.
- **.gitignore:** drop LaTeX artifacts; ignore `referee_reports/.work/` and (optionally) input PDFs.

## Step 5 — VERIFY
- Structural: all remaining skill/agent frontmatter valid; **grep for orphaned references** to any removed skill/agent/rule/folder in surviving docs — fix all.
- Functional: no test PDF is present, so I'll add a short synthetic sample paper to `papers/` and run `/referee` on it end-to-end to confirm the pipeline produces a well-formed report. (Sample removed or kept per your preference.)

## Step 6 — COMMIT (local only)
Logical commits on `main`: `checkpoint` → `remove: strip slide/teaching infra` → `feat: referee pipeline (agents, skill, rules)` → `docs: rewrite README/CLAUDE/MEMORY for peer review`. **Will not push** without your explicit request.

---

## Proposed final structure
```
├── CLAUDE.md · README.md · MEMORY.md · LICENSE · .gitignore
├── papers/                 # INPUT: drop PDFs here
├── referee_reports/        # OUTPUT: generated reports (+ .work/ scratch)
├── quality_reports/{plans,session_logs}/
├── templates/{referee-report,requirements-spec,session-log,quality-report}.md
└── .claude/
    ├── skills/{referee,commit,context-status,learn,lit-review}/
    ├── agents/{methods,contribution,literature,writing}-reviewer.md, adversarial-referee.md
    ├── rules/{referee-report-protocol,review-verification,pdf-ingestion,orchestrator-protocol,
    │          plan-first-workflow,quality-gates,session-logging,meta-governance}.md
    ├── hooks/  (unchanged except verify-reminder pruning)
    └── settings.json (pruned permissions)
```

## Open questions for you
1. **Sample paper:** OK to add a short synthetic econ paper under `papers/` to prove the pipeline works, then leave it as an example (or delete it)? *(default: add + keep as example)*
2. **Web access for the literature reviewer:** allow it to use WebSearch/WebFetch to catch missing citations, or keep it offline (judge only from the paper's own reference list)? *(default: allow web, clearly flagged)*
