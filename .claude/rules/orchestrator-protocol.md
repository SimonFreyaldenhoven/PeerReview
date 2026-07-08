# Orchestrator Protocol: Contractor Mode

**Once a paper is supplied (or a plan approved), the orchestrator runs the review end-to-end and hands back a finished report.** Like a contractor: you approve the job, it plans, executes, checks its own work, and presents the result.

## The Referee Loop

```
Paper supplied → orchestrator activates
  │
  Step 1: INGEST — read the PDF end-to-end; write the paper brief
  │        (pdf-ingestion.md)
  │
  Step 2: REVIEW — launch the 5 specialists in parallel
  │        (methods · contribution · literature · writing · consistency)
  │
  Step 3: ADVERSARIAL — toughest-referee pass over paper + specialist outputs
  │
  Step 4: SYNTHESIZE — dedupe, reconcile, rank, rate, recommend → one report
  │        (referee-report-protocol.md)
  │
  Step 5: FACT-CHECK — verify every claim against the paper
  │        (review-verification.md); fix → re-check
  │
  Step 6: GATE — apply the referee-report ship checklist
  │        (quality-gates.md)
  │
  └── All gates pass?
        YES → save report, present Summary Assessment + recommendation
        NO  → loop back to Step 4 (max 5 rounds)
              After max rounds → deliver with remaining gaps flagged
```

## Limits

- **Main loop:** max 5 synthesize–factcheck rounds
- **Verification retries:** max 2 attempts per unresolved claim
- Never loop indefinitely; if a claim can't be verified, drop or soften it (don't stall)

## "Just Do It" Mode

When the user says "just referee it" / "handle it":
- Skip any confirmation pause; run the full pipeline
- Save the report automatically once all gates pass
- Still present the Summary Assessment and recommendation

## When to plan first

A single paper → single report is a well-scoped job; run the pipeline directly, no plan needed. Enter plan mode (`plan-first-workflow.md`) only for non-trivial *meta-work* on this repo — changing the pipeline, adding reviewer lenses, or reshaping the infrastructure.
