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

For each **retrieve** source, which agentic moves apply? (This is what separates modern agentic retrieval from naive "embed → top-k → stuff".)

**Five moves:**
- **Routing:** Which source to query?
- **Document grading:** Is what I retrieved actually relevant?
- **Reranking:** Reorder results by relevance.
- **Self-verification:** Did the update use the retrieved evidence? Did Cortex ground claims in retrieved data?
- **Caching:** Cache results to avoid re-retrieving the same data.

**Applied to our retrieve sources:**

| Source | Moves | Why |
|---|---|---|
| `get_activity` | Document grading + self-verification | Grade for relevant activity only (this week's PRs/issues, not all history). Verify Cortex cited the activity it retrieved (e.g., PRs #820, #823 are real). |
| `get_roadmap` | Document grading + self-verification | Grade retrieved roadmap for Northstar-specific items (don't include unrelated initiatives). Verify Cortex cited roadmap items correctly without leaking confidential content. |
| `get_norms` | Reranking + self-verification | Rerank norms by relevance to status-update task (format/tone rules first, sprint-planning rules lower). Verify Cortex followed the norms in the draft (no unconfirmed dates, no overcommitments, format matches team style). |

## 4. Memory map (your PM brain)

| Memory type | What Cortex stores | Scope / TTL |
|---|---|---|
| **Working** (in-loop) | Current task brief + retrieved activity/roadmap/norms + status update draft in progress | This run only (~1 hour); discarded after draft is sent to PM |
| **Episodic** (past runs) | Past activity history (PRs, issues) + past decisions (what was flagged red, escalated, resolved) | 3 months (rolling window; helps spot trends & context) |
| **Semantic** (durable facts/prefs) | Team norms + preferences + key stakeholders | Refresh each run (via `get_norms` retrieve); stakeholders updated on change (event-driven) |
| **Shared** (across agents) | Draft + source data (what was pulled) + critic feedback + revisions | 30 days after report is sent (reference window for questions; then archived) |

## 5. Memory risks & mitigations

| Risk | Where it bites | Mitigation |
|---|---|---|
| **Drift** | Norms change, but Cortex uses stale rules (wrong format, outdated policy). Roadmap facts drift as projects ship. Key stakeholders change. | Norms: refresh each run via `get_norms` retrieve. Roadmap: update periodically (weekly with sprint cycle). Stakeholders: update on change (event-driven). |
| **Poisoning** | Bad data in source (false metric, fake PR, corrupted issue). Cortex retrieves it, cites it, pollutes the draft. | Self-verification (Step 2): Cortex grounds claims in retrieved data. Critic + PM review catch mismatches or false citations. |
| **Staleness** | Old activity (closed PRs from Q2), outdated decisions (resolved escalations) clog retrieval, make context irrelevant. | Bounded retrieval window: `get_activity` retrieves only this week (7 days). Document grading: filter for relevant activity only, discard old/closed work. |
| **PII / retention** | Activity, updates, norms may contain PII (email addresses, personal notes in issues). Retained too long = compliance risk. | Filter at retrieval time: only de-PII'd data passed to Cortex. Enforce TTLs: 30 days for shared drafts, 3 months for episodic history. Purge older data. |
