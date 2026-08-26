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

- **Runtime:** _managed agent platform · serverless · self-hosted, and why_
- **Operator / on-call owner:** _who owns it in production_
- **Rollback:** _how you turn it off / revert_
- **Monitoring:** _the dashboard + the signals you watch_

## ROI metrics (beyond adoption & tokens)

| Metric | Target |
|---|---|
| _Task completion rate_ | _…_ |
| _Time saved / cost-to-serve_ | _…_ |
| _Trust incidents_ | _…_ |

## Widen-autonomy decision rule

_What evidence lets you turn the dial up one notch, stated in advance._

## Governance & forward strategy

- **Compliance:** _what data must never enter a prompt; how PII is handled_
- **Safety:** _which actions stay above the agent line for everyone; kill switch_
- **Reliability:** _cost/iteration caps; escalate-on-stuck; fallback if the model is down_
- **Strategy:** _the next segment or capability you'd widen into, and the eval that gates it_
