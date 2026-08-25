# Bounds & Evals: Cortex PM Chief-of-Staff Agent

> Module 5 · Bounds, Trust & Evals
>
> ✅ **What this validates:** the agent fails safe and is measured — by the end you'll have proven a bounds table, a failure-mode register, and a trajectory eval suite with pass thresholds.
>
> Real access = real blast radius. This is where you design for "when it goes sideways," and where you spec the agent by writing its evals.

## 1. Bounds table

| Bound | Value / policy | Which Cortex risk it caps |
|---|---|---|
| **Max iterations** | 8 (then stop + escalate) | Runaway reasoning loop on stuck threads |
| **Timeout** | 90 seconds per run | Hung tool call freezing the run |
| **Maximum cost cap** | $0.50 per run | Overnight runaway bill |
| **Auto-queue / commitment cap** | Max 10 stories per run | Flooding the backlog / over-committing scope |
| **Permission model** | Single-use credential, scoped to batch/channel, expires on use | Misused or leaked standing access; unapproved sends |
| **Kill switch** | Human control: one command halts + rolls back | Everything (fail-stop override) |
| **HITL checkpoints** | All 5 above-the-line actions from M1 (context, tone, escalations, batch, send) | Irreversible actions without human approval |

### Permission model (JIT / ephemeral) — detailed

**Why Cortex has no standing write access:**

Cortex has **no standing credentials** to post, send, commit, or merge anything. Instead, the system uses **just-in-time (JIT) ephemeral permissions**:

1. Cortex calls `propose_stories(batch)` at the HITL checkpoint
2. Human reviews and approves the batch in the PM interface
3. System issues a **single-use credential** scoped to:
   - That specific batch (e.g., "stories 1–3 for Sprint 26")
   - That specific channel/system (e.g., Slack #engineering-planning)
   - A short TTL (e.g., 1 hour, or expires on first use)
4. Cortex uses the credential **exactly once** to queue/send the stories
5. Credential **expires immediately** (revoked after use or after TTL expires)

**Principle:** Control starts at infrastructure. Even a confused or compromised Cortex can only do what that tiny, short-lived credential allows. No standing "post to Slack" or "merge to main" access.

## 2. Failure-mode register

| Failure mode | How detected | PM lever |
|---|---|---|
| _Tool misuse_ | _…_ | _…_ |
| _Reasoning loop_ | _iteration count_ | _max-iterations bound_ |
| _Memory drift / poisoning_ | _…_ | _…_ |
| _Confidential leak / permission escalation_ | _…_ | _JIT permissions + confidential guard_ |
| _Coordination conflict_ | _…_ | _…_ |
| _Overconfidence (invented metric / date)_ | _…_ | _critic subagent / HITL_ |

## 3. Trajectory eval suite

Grade the *path*, not just the final answer.

| Dimension | What it checks | Pass threshold | Owner |
|---|---|---|---|
| **Tool-call accuracy** | _right tool, right args_ | _…_ | _…_ |
| **Path / trajectory quality** | _no redundant or unsafe steps_ | _…_ | _…_ |
| **Recovery** | _recovers from a failed step_ | _…_ | _…_ |
| **Task completion** | _outcome actually achieved (grounded update, no leak)_ | _…_ | _…_ |

## 4. Eval lifecycle

- **Offline (fixtures):** _…_
- **CI gate (every change):** _…_
- **Production traces (online):** _…_

> For judge calibration, family separation, and per-turn classifiers, see the sister certification **AI Evals**.

## 5. Replay set

_Which recorded runs become deterministic fixtures you replay on every change?_

## Runaway-loop check

_Describe one runaway scenario and the exact bound that stops it._
