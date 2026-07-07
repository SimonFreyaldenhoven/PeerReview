# Meta-Governance: This Repository's Dual Nature

**This repository is BOTH a working referee setup AND a template others fork.** Keeping the two straight decides what to commit, what to keep local, and how to write documentation.

---

## The Two Identities

### Identity 1: Working Setup
- You review real manuscripts and accumulate learnings about *your* refereeing (domain conventions, journals you referee for, recurring issues you look for).
- You iterate on the pipeline (reviewer lenses, report format, prompts).

### Identity 2: Public Template
- Others fork this to referee papers in *their* field. The domain defaults are economics/econometrics, but a forker may work in finance, poli-sci, or applied stats.
- They need generic, adaptable patterns — not your specific journal preferences hard-coded in.

---

## Decision Framework: "Generic or Specific?"

**GENERIC (commit — helps everyone):**
- The pipeline shape (ingest → specialists → adversarial → synthesize → fact-check).
- Reviewer-lens structure, the report protocol, the anti-fabrication rules.
- Workflow patterns (contractor mode, quality gates, plan-first for meta-work).

**SPECIFIC (keep local / gitignored):**
- Journals you referee for and their idiosyncratic bars.
- Personal shorthand, your standing pet-peeve checks.
- Machine paths, API keys, local tool quirks.

When unsure: *"Would someone forking this to referee papers in another field benefit?"* Yes → commit. No → keep local.

---

## Two-Tier Memory

### MEMORY.md (root, committed)
Generic learnings that improve the pipeline for everyone. Tagged `[LEARN:category] wrong → right`. Keep under ~200 lines (it loads into context). Review after significant sessions.

### .claude/state/personal-memory.md (gitignored, local)
Machine- and user-specific learnings — your journals, your domain tweaks, local paths. No size limit; never syncs.

---

## Adapting the Template (for forkers)

The defaults are economics/econometrics. To retarget the domain:
- Edit the reviewer lenses in `.claude/agents/*-reviewer.md` (e.g. swap "identification strategy" for your field's core validity questions).
- Adjust the dimensions and rubric in `.claude/rules/referee-report-protocol.md`.
- Update the project identity and skill table in `CLAUDE.md`.

Keep changes framework-oriented: describe *what a reviewer lens is for*, then give a domain example — don't hard-code one field as the only option.

---

## Dogfooding

Follow the patterns this repo recommends:
- **Fact-check before shipping** — never let an ungrounded claim into a report (`review-verification.md`).
- **Gate before saving** — the referee-report checklist must pass (`quality-gates.md`).
- **Plan-first for meta-work** — changing the pipeline is a plan-mode task; refereeing one paper is not.
- **Context survival** — keep `[LEARN]` entries current in MEMORY.md and session logs on disk before compression.

---

## Amendment Process

When a better generic/specific distinction emerges or the pipeline changes: propose in a session log or plan, update this file, and record a `[LEARN:meta-governance]` entry in MEMORY.md.
