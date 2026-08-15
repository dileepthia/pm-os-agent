# Loop Spec: Cortex PM Chief-of-Staff Agent

> Module 2 · Loop Engineering, ★ Deliverable 2
>
> ✅ **What this validates:** the agent knows when to run and when to stop — by the end you'll have proven a one-page Loop Spec with a trigger, a definition of "done," and explicit stop conditions.
>
> Your one-page blueprint for how the work you handed to the agent (M1) actually *runs*.
> An agent is just a prompt that fires itself, this spec says when it fires, what "done" means, and what it needs to do the job. Living document; refine as the course progresses.

## 1. Trigger & loop type

**Chosen type:** Cron (scheduled weekly)

**Trigger:** Weekly at a fixed time (e.g., Monday 9am UTC)

**Why:** Status updates are periodic events sent at predictable intervals. A cron schedule is resource-efficient and aligns with sprint cadence, eliminating the need for reactive triggers (hook) or resource-wasteful polling (heartbeat).

**Idempotency:** If cron fires twice, check the last run timestamp (stored in state). Skip if another run completed within the last 24 hours.

**Ruled out:**
- Hook: No external trigger; updates are internally scheduled
- Heartbeat: Wasteful; we only need status at send time
- Goal: Not applicable; success = draft completed, not iterative validation

## 2. Goal / definition of done

Draft status update written, validated by critic, and queued for human approval. Cortex never posts; human reviews before send. Either all conditions are successfully met by Cortex, or they are escalated and passed to human via HITL checkpoint.

## 3. Stop conditions

| Condition | What it looks like | What happens |
|---|---|---|
| **Success** | Critic validates draft. All data pulled. Draft passes validation or is queued for human approval at HITL checkpoint. | Loop exits cleanly. Draft saved to `run-output/`. |
| **Stuck / give up** | Cannot pull project state from primary source after 3 retries. | Log error. Stop loop. Escalate to human with context. |
| **Escalate to human** | Embargoed/confidential content detected, OR story batch exceeds 10-item cap, OR public GA-date commitment mentioned. | Hold draft. Flag for human review at HITL checkpoint. Do not queue for send. |

## 4. State

Last run timestamp (for 24-hour dedupe check). Previous 3 weekly updates (tone, structure, format, language reference). List of monitored projects.

## 5. The five things a loop can lean on

| Component | For Cortex |
|---|---|
| **Work tree** (isolated workspace per run, a git worktree) | YES — isolated run per week. Allows rollback to any week's version if needed. |
| **Skills** (reusable capabilities) | Data Quality Checker (validate pulled project state). Content Policy Checker (detect embargoed content). |
| **Plugins / connectors** (tools & access, optional if you don't have one yet) | Pre-flight check before each run: Jira accessible? Slack accessible? Calendar accessible? Fail gracefully if a connector is down. |
| **Subagents** (independent check when the loop can't grade itself) | Critic (in agent.py) validates draft before queueing. Sufficient for M2; escalate to independent validator in M3+ if needed. |
| **State tracking** | Always-on. Tracks: last run timestamp, previous 3 updates, monitored projects list. |

> Context plan (M4) and the hand-off to bounds & evals (M5) come in later modules — you'll add them to their own deliverables then, not here.

## Link to live loop

_[path to your agent in `00-build/`]_
