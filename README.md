# Cortex PM Chief-of-Staff Agent

> An AI agent that drafts weekly executive summary report with human approval gates

_Dileep Thiagarajan · Agentic Loops for PMs Cohort · August 2026_

Repo: https://github.com/dileepthia/pm-os-agent

This repo is my final project for the Agentic Loops for PMs Certification, **Cortex PM Chief-of-Staff Agent**. Each module’s artifact lives in its own folder; this README is the dashboard and the pitch.

---

## Module artifacts

### M1 · The Agent Line
- **Agent-line map**: [`01-agent-line/agent-line-map.md`](01-agent-line/agent-line-map.md)

### M2 · Loop Engineering
- **Loop spec**: [`02-loop-design/loop-spec.md`](02-loop-design/loop-spec.md)

### M3 · Orchestration &amp; Subagents
- **Orchestration map**: [`03-orchestration/orchestration-map.md`](03-orchestration/orchestration-map.md)

### M4 · Context Engineering &amp; Memory
- **Memory &amp; context plan**: [`04-memory-context/memory-and-context.md`](04-memory-context/memory-and-context.md)

### M5 · Bounds &amp; Evals
- **Bounds &amp; evals**: [`05-bounds-evals/bounds-and-evals.md`](05-bounds-evals/bounds-and-evals.md)

### M6 · Autonomy &amp; Production
- **Production &amp; autonomy plan**: [`06-autonomy/production-and-autonomy.md`](06-autonomy/production-and-autonomy.md)
- **Prototype write-up**: [`06-autonomy/prototype.md`](06-autonomy/prototype.md)

---

## Ship plan

### Autonomy dial (per segment)
Exec stakeholder — Bounded-autonomous (downstream of HITL checkpoint; PM approval before they see it)
Engineering leads — Supervised (review after Cortex sends; loop back to PM if adjustments needed)
Technical Pgm — Bounded-autonomous (FYI only; visibility for planning)

### Trust Ladder rung + eval gate
Current rung: Assisted (Cortex drafts, PM approves every time before send)

Eval gate to next rung (Supervised):

≥99% overall accuracy
0 confidential leaks
EV-1 (grounding) ≥99% pass rate
Within cost bounds (EV-5: ≤$0.50/run)
Measured over 90 days
Incident record: 0 major, ≤1 minor

### Deployment plan
Runtime: Serverless (AWS Lambda), weekly cron

On-call owner: Dil (PM Lead); escalate to Jim (PM Director) if unavailable

Rollback: Drop dial from Assisted to Shadow (read-only) if safety issue (leak/jailbreak/cost overrun). Critical failure: disable Lambda.

Monitoring:

Eval pass % ≥99% (alert if below)
Escalation rate ≤5% (alert if above)
Page Dil if: run fails, eval gates fail, leak detected, cost overrun, empty report

### ROI metrics + widen-autonomy rule
ROI metrics (beyond adoption & tokens):

Manual edits reduced: <5 edits/week
Exec satisfaction: ≥4/5 survey score
Cost-to-serve: ≤$0.50/run
Trust incidents: 0 leaks/report

Widen-autonomy rule:
Move from Assisted to Supervised when: Cortex passes all eval gates (≥99% grounding, 0 leaks, cost ≤$0.50/run) for 12 consecutive runs with zero major incidents.

### Governance &amp; strategy
Compliance: Launch dates, strategic plans (M&A, layoffs), embargoed roadmap NEVER enter prompt. Enforced at retrieval (get_roadmap/get_norms filtered for public only).

Safety: All above-the-line actions (posting, marking gates, committing dates) stay HUMAN always. Kill switch: Dil can shut down; Jim if unreachable.

Reliability: Cost cap $0.50/run, iteration cap 8, revision cap 2. Escalate-on-stuck after 3 retries. Model-down: retry after 1hr, never use cached draft.

Strategy: Next segment = other PMs. Gate: 12 consecutive runs pass all eval gates with zero major incidents. Then move them from Assisted to Supervised.

---

## Build insights

- **Friction point.** The hardest part was understanding the agent line — figuring out what actions stay above the line (human-only, never delegated) versus what can go below (candidate for autonomy). It's easy to say "only humans send updates" in theory; actually enforcing it in code and bounds is different. Every tool call is a choice.
- **Key learning.** Key #1: Agents with proper evals offload real work. Cortex drafts, validates, and escalates — but the human keeps the approval gate. That's the wedge between "demo agent" and "shipped agent." The evals make it credible.  Key #2: Start with full human control, then dial autonomy gradually. Assisted → Supervised → Bounded-Autonomous. Each rung has a gate. You don't go from "human presses send" to "agent posts overnight" in one step. Autonomy is a product decision per user segment, not a one-time toggle.  Key #3: Memory context design was a surprise. Deciding what to retrieve fresh (activity, roadmap) vs. what to keep long-context (task, past updates) changed how I think about agent reasoning. Staleness is a failure mode, not just a theory. Retrieval is cheap; hallucination is expensive.
- **Aha moment.** The autonomy dial unlocked it for me. Once I saw that different users can have different autonomy levels on the same agent — and that each level needs its own eval gate to climb — everything else made sense. Ship the same agent to your power users at Supervised while keeping new users at Assisted. No forking, no "lite" version.

---

_Certification submission, Agentic Loops for PMs Certification._
