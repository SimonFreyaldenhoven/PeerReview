# Referee Report Protocol

Defines the structure, tone, and rating rubric for every report produced by `/referee`. The goal is a report an editor at a top economics journal could act on directly.

## Tone

- **Constructive but rigorous.** Every criticism is paired with a suggestion or the specific analysis that would resolve it. Harsh on the work, respectful to the authors.
- **Specific, not generic.** Reference exact sections, equations, tables, figures, pages. "The identification is weak" is useless; "the parallel-trends assumption (Sec. 4.2) is untested — no pre-trend plot is shown for the 2015 cohort" is a referee comment.
- **Calibrated.** A major concern is one where the *conclusions change* if it's right. A minor concern is a fix that improves the paper but doesn't threaten its claims. Do not inflate minors into majors.
- **Honest about uncertainty.** If something couldn't be verified from the PDF, say so; don't assert it.

## Recommendation scale

- **Accept** — publishable essentially as is (rare).
- **Minor Revision** — small fixes, no new analysis needed.
- **Major Revision (R&R)** — the contribution is promising but key concerns must be resolved with additional work.
- **Reject & Resubmit** — fundamental rework needed, but a future version could be viable.
- **Reject** — a fatal flaw the current design cannot overcome.

## Report structure (structured-critique format)

```markdown
# Referee Report: [Paper Title]

**Date:** YYYY-MM-DD
**Manuscript:** [path/filename]
**Prepared by:** /referee pipeline

## Summary Assessment
**Recommendation:** [Accept / Minor Revision / Major Revision / Reject & Resubmit / Reject]

[2–3 paragraphs: the paper's question and contribution in the referee's own words, the
main strengths, and the decisive concerns that drive the recommendation.]

## Strengths
1. …
2. …
3. …

## Major Concerns
### MC1: [title]
- **Dimension:** Identification / Econometrics / Contribution / Literature / Writing
- **Issue:** [what and why it threatens the conclusions]
- **Evidence:** [exact location]
- **Suggestion:** [fix or analysis that would resolve it]

[MC2, MC3, … — most severe first; lead with any FATAL adversarial objection]

## Minor Concerns
### mc1: [title]
- **Issue:** [description] — **Location:** […]
- **Suggestion:** [fix]

## Referee Objections
The hardest questions the authors must be able to answer:
### RO1: [pointed question]
- **Why it matters:** [why it could be decisive]
- **How to address it:** [response or additional analysis]

[3–5 objections]

## Specific Comments
[Optional section/line-level notes: typos, notation, table fixes, by location.]

## Ratings
| Dimension | Rating (1–5) |
|-----------|:------------:|
| Contribution / Importance | N |
| Identification | N |
| Econometric Specification | N |
| Literature & Positioning | N |
| Writing & Presentation | N |
| **Overall** | **N** |
```

## Rating rubric (1–5, per dimension)

- **5** — Exemplary; nothing a referee would insist on.
- **4** — Strong; minor improvements only.
- **3** — Credible but with real, addressable gaps.
- **2** — Serious problems that must be fixed before the claim holds.
- **1** — The dimension undermines the paper.

The **Overall** rating and the recommendation are the referee's judgment informed by the dimensions — **not** a mechanical average. A single fatal identification problem caps Overall regardless of how polished the rest is.
