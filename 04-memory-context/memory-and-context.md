# Context Engineering & Memory: Cortex PM Chief-of-Staff Agent

> Module 4 · Context Engineering & Memory
>
> ✅ **What this validates:** the agent reasons on the right, safe inputs — by the end you'll have proven a context budget, per-source retrieve-vs-long-context decisions, and a memory map with risk mitigations.
>
> 🗂️ **How the lab maps to this file:** In **Part A** (before the lecture) you don't edit this file — you rough-draft on scratch, focused on the per-source calls in **section 2** plus a quick remember/forget + "how it rots" sketch. In **Part B** (after the lecture) you complete **all five sections**; the Lab Guide's guided builder writes this file for you to copy in and commit.

## 1. Context budget

**Priority order (what Cortex always sees):**
1. **Long-context first:** Task brief (`get_task`) + last 3 past updates (`search_past_updates`)
2. **Retrieve on-demand:** Activity (volatility), roadmap (confidential gating), team norms (freshness)

**Rationale:** Keep the task and tone reference always in view (latency matters; they're stable). For everything else — activity, roadmap, norms — retrieve fresh to avoid staleness and manage confidential content filtering. This keeps the base context window tight while ensuring Cortex always has current, audited data.

## 2. Retrieve vs. long-context: per source

For each data source, decide: **retrieve** (narrow a large/changing corpus to the relevant slice) or **long-context** (just include a bounded set you can reason over).

| Source | Size / volatility | Decision | Deciding factor | Why |
|---|---|---|---|---|
| `get_task` | Bounded, static | Long-context | Latency | Task brief is always in view; no retrieval round-trip needed. |
| `search_past_updates` | Unbounded, stable | Long-context | Latency | Keep last 3 updates in system prompt for tone/format reference; no retrieval overhead. |
| `get_activity` | Large, changes daily | Retrieve | Volatility | Activity changes daily; can't bake into long-context or it goes stale. Retrieve ensures fresh PRs, issues, Sev-1s. |
| `get_roadmap` | Medium, slow-changing | Retrieve | Citation/audit | Roadmap contains confidential items; filter at retrieval time so Cortex only sees non-confidential public items (principle of least privilege). |
| `get_norms` | Medium, must stay current | Retrieve | Volatility | Team norms can change; can't hardcode stale rules. Retrieve ensures Cortex always operates on current playbook. |

## 3. Retrieval quality plan

_Which of these apply, and how? (This is what separates modern agentic retrieval from naive "embed → top-k → stuff".)_

- **Routing**: _which source to query?_
- **Document grading**: _is what I retrieved actually relevant?_
- **Reranking**: _…_
- **Self-verification**: _did the update use the retrieved evidence?_
- **Caching**: _…_

## 4. Memory map (your PM brain)

| Memory type | What Cortex stores | Scope / TTL |
|---|---|---|
| **Working** (in-loop) | _…_ | _this run_ |
| **Episodic** (past runs) | _past status updates, decisions_ | _…_ |
| **Semantic** (durable facts/prefs) | _team norms, roadmap facts_ | _…_ |
| **Shared** (across agents) | _…_ | _…_ |

## 5. Memory risks & mitigations

| Risk | Mitigation |
|---|---|
| _Drift_ | _…_ |
| _Poisoning_ | _…_ |
| _Staleness_ | _…_ |
| _Confidential / retention_ | _scoping + flags (Cortex touches embargoed roadmap)_ |
