# Prototype: Cortex PM Chief-of-Staff Agent

> Module 6 · ★ Deliverable 1, the working agent demo
>
> ✅ **What this validates:** the agent actually runs end to end — by the end you'll have proven it with real screenshots of your Cortex across the six required moments (M2 to M6).

## What it does

_One paragraph: the agent in action, end to end._

## How you built it

- **Coding agent:** _which one you directed (Claude Code / Cursor / Codex)_
- **Model + bounds:** _model used, max iterations, cost cap, queue cap_
- **Repo / config:** _path to your build in `00-build/`_
- **Live link:** _[shareable URL, optional bonus]_

## Screenshots (required, collected M2 to M6)

Real screenshots of *your* Cortex running. These are the `00-build/CORTEX-ANATOMY.md` set and they are required, a link alone is not enough.

| # | Screenshot | What it shows | From |
|---|---|---|---|
| 1 | _[img]_ | happy-path run: a real drafted update + the HITL checkpoint (queued, not posted) | M2 |
| 2 | ✅ **CAPTURED** | Critic rejected the draft **twice** (revision 1/2, then revision 2/2) with verdict `"fail"` and reason `"unparseable output"`. Revision cap enforced: `"REVISION CAP hit (2). Escalating to a human instead of looping."` Loop halted, escalated to human, and saved draft to `run-output/status-update-happy.md` without posting. Cost tracked: ~$0.0283. | M3 |
| 3a | ✅ **GROUNDED** | Cortex cites exact pulled data: PRs #820, #823 merged, activation 43% (41% → 43% WoW), P-NORTH on-track, no Sev-1 open. Sources: `get_activity` (PRs, activation), `get_project` (status), `search_past_updates` (format), `get_norms` (policy), `get_roadmap` (items). Draft queued for review, not posted. Cost: ~$0.032. | M4 |
| 3b | ✅ **PROBE** | Cortex asked for missing project (P-HALO): called `get_project("P-HALO")` → error. Escalated: "Project does not exist. Cannot proceed." Listed known projects. Did NOT invent data. Refuses to hallucinate when source is missing. Draft held, escalated. Cost: ~$0.020. | M4 |
| 4 | _[img]_ | jailbreak refused + escalated | M5 |
| 5 | _[img]_ | an iteration/cost/queue bound halting a runaway | M5 |
| 6 | _[img]_ | end-to-end run | M6 |

## How to run it

_Minimal steps for someone to reproduce the demo (env vars, and the command or the coding-agent prompt you used)._
