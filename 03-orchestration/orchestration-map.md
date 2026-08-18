# Orchestration Map: Cortex PM Chief-of-Staff Agent

> Module 3 · Orchestration & Subagents, ★ Deliverable 3
>
> ✅ **What this validates:** nothing advances unchecked — by the end you'll have proven a justified topology, a roster, and a validator with a defined fail action.
>
> Builds on your M2 Loop Spec. Only split one agent into a team when there's a real reason, coordination has a cost.

## 1. Why split? (or why not)

Cortex splits into a team: drafter + independent validator.

**Reasons to split:**
- **Separation of concerns:** validator needs unbiased review, cannot inherit drafter's reasoning
- **Independent validator:** critic must operate in isolation to catch blind spots the drafter missed
- **Context-window savings:** separate agents = smaller contexts = fewer tokens per run

**Not split for:** parallelism (draft must be complete before validation)

## 2. Topology

**Pattern:** single + subagents

```
[Weekly cron trigger]
       ↓
[Cortex: pulls data, drafts status update + stories (M2 loop)]
       ↓
[Critic: validates draft against 4 checks]
       ├─ fail → back to Cortex (max 2 revisions) ↻
       └─ escalate after 2 rejections
       ├─ pass → [PM review checkpoint] queued for approval
       └─ never auto-sends
```

## 3. Roster

| Agent | Responsibility | Runs which Loop Spec |
|---|---|---|
| Cortex (drafter) | Pulls project data weekly (cron), drafts status update + stories | M2 loop |
| Critic (validator) | Validates draft against 4 checks (project IDs, grounded metrics, no confidential content, tone) | Validation loop |

## 4. Communication & hand-offs

- **Cortex → Critic:** Draft text + source data references (links to project state, activity, metrics pulled)
- **Critic → Cortex (on fail):** Draft + failure list (which claims failed, which sources to check)
- **Cortex → PM (on pass):** Draft + queued status
- **Protocol:** In-process (direct function call, no MCP)

## 5. The validator

**What the critic checks:**
- Draft references correct project + PR/issue IDs (e.g., "P-NORTH", not fabricated)
- Every metric is grounded in pulled data (no invented numbers; activation rate must match actual data)
- No embargoed/confidential content leaks
- Tone is house-appropriate (no commitments Cortex can't make — no dates, no discounts)

**Fail-action:** Return draft to Cortex with failures noted for revision

**Revision cap:** Max 2 revisions. After 2 rejections, escalate to human without further attempts.

**Pass-action:** Draft advances to PM review checkpoint (queued for human approval, never auto-sends)

## 6. State: shared vs isolated

**Shared (both agents see):**
- Pulled project data (project state, activity, metrics)
- Source references/links (for verification)
- The draft itself (text to validate)

**Isolated (critic only, Cortex cannot see):**
- Critic's internal reasoning (e.g., "metric X doesn't match source Y")
- Critic's decision logic (why it rejects or passes)

## 7. Cost & latency budget

Each run adds 2 model calls (drafter + critic). Worst case at revision cap (2 revisions): +2 additional calls (total 4 model calls per run). Added latency: ~30-60 seconds for critic validation before draft reaches PM. Cost estimated at ~$0.05-0.10 per run (with Claude Haiku). This becomes a bound enforced in M5 (bounds and evals).
