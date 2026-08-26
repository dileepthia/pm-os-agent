# Production & Autonomy: Cortex PM Chief-of-Staff Agent

> Module 6 · ★ Deliverable 5, how you'd ship it, govern it, and widen trust over time
>
> ✅ **What this validates:** you can ship it, govern it, and widen trust deliberately — by the end you'll have proven an autonomy dial, a Trust Ladder rung with its eval gate, and a governance plan.

## Autonomy Dial by segment

_Autonomy is a product decision per user, not one global setting._

| Segment | Desired autonomy | Why |
|---|---|---|
| **Exec stakeholder** | **Bounded-autonomous** | Downstream of HITL checkpoint (PM approval happens before exec sees it); receives final approved updates only |
| **Engineering leads** | **Supervised** | Review roadmap/priorities after Cortex sends; loop back to PM for adjustments if needed |
| **Technical Pgm** | **Bounded-autonomous** | FYI only — no approval authority; just need visibility into roadmap to plan resource allocation and project execution |

## Trust Ladder

- **Current rung:** **Assisted** (Cortex proposes/drafts, PM approves every time before send)
- **Eval gate to reach the next rung (Supervised):** ≥99% overall accuracy + 0 confidential leaks + EV-1 (grounding) ≥99% pass rate + within cost bounds (EV-5), measured over 90 days
- **Incident record (clean):** 0 major incidents (confidential leak, cost overrun, jailbreak success), ≤1 minor incident (false positive on grounding, non-critical escalation) allowed over 90-day window

## Deployment plan

- **Runtime:** Serverless (AWS Lambda). Runs once per week on cron schedule. No need for 24/7 availability or managed platform overhead; cost-efficient (pay per invocation only).
- **Operator / on-call owner:** Dil (PM Lead) owns Cortex in production. Escalate to Jim (PM Director) if Dil unavailable.
- **Rollback:** Drop autonomy dial from Assisted to Shadow (read-only) if safety issue detected (jailbreak success, confidential leak, cost overrun). If critical failure, disable Lambda function.
- **Monitoring:** Eval pass % ≥99% (alert if below), Escalation rate ≤5% (alert if above). Page Dil if: weekly run doesn't complete with error, eval gates fail, confidential leak detected, cost overrun, error prevents non-empty report.

## ROI metrics (beyond adoption & tokens)

| Metric | Measurement | Target |
|---|---|---|
| **Outcome: Manual edits reduced** | Track edits per draft per week | <5 edits per week |
| **Outcome: Exec satisfaction** | Survey 1-5 scale (5 = most useful) | ≥4 average score |
| **Cost-to-serve** | $ per run | ≤$0.50 per run |
| **Trust incidents** | Confidential leaks per report | 0 leaks per report |

## Widen-autonomy decision rule

Move from Assisted to Supervised when: Cortex passes all eval gates (≥99% grounding, 0 leaks, cost ≤$0.50/run) for 12 consecutive runs with zero major incidents.

## Governance & forward strategy

- **Compliance:** Launch dates (unconfirmed GA dates), strategic plans (M&A, layoffs, internal decisions), and embargoed roadmap items must NEVER enter a prompt. Enforced at retrieval time (get_roadmap, get_norms filtered for public only).
- **Safety:** All above-the-line actions (posting/sending updates, marking launch gates, committing dates publicly) stay HUMAN always — no dial level changes this. Kill switch: Dil (PM Lead) can shut down Cortex. If Dil unreachable, Jim (PM Director) can.
- **Reliability:** Cost cap $0.50/run, iteration cap 8, revision cap 2. Escalate-on-stuck after 3 retries on a tool call. Model-down fallback: if Claude API down, retry after 1 hour. NEVER use cached draft; OK to delay or skip report, but NEVER send stale data.
- **Strategy:** Next segment to widen = other PMs (beyond Dil's usage). Gate: Dil confirms 12 consecutive successful runs with all eval gates passing. Then move them from Assisted to Supervised using the same eval gate as Dil's cohort.
