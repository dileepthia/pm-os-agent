# Build Insights: Cortex PM Chief-of-Staff Agent

> Module 6 · ★ Deliverable 4, what you learned building it
>
> ✅ **What this validates:** you can reflect on what building it taught you — by the end you'll have proven the friction, the learning, and the aha that changes how you'd design your next agent.

## Friction

The hardest part was understanding the agent line — figuring out what actions stay above the line (human-only, never delegated) versus what can go below (candidate for autonomy). It's easy to say "only humans send updates" in theory; actually enforcing it in code and bounds is different. Every tool call is a choice.

## Learning

**Key #1: Agents with proper evals offload real work.** Cortex drafts, validates, and escalates — but the human keeps the approval gate. That's the wedge between "demo agent" and "shipped agent." The evals make it credible.

**Key #2: Start with full human control, then dial autonomy gradually.** Assisted → Supervised → Bounded-Autonomous. Each rung has a gate. You don't go from "human presses send" to "agent posts overnight" in one step. Autonomy is a product decision per user segment, not a one-time toggle.

**Key #3: Memory context design was a surprise.** Deciding what to retrieve fresh (activity, roadmap) vs. what to keep long-context (task, past updates) changed how I think about agent reasoning. Staleness is a failure mode, not just a theory. Retrieval is cheap; hallucination is expensive.

## Aha moment

The autonomy dial unlocked it for me. Once I saw that different users can have different autonomy levels on the same agent — and that each level needs its own eval gate to climb — everything else made sense. Ship the same agent to your power users at Supervised while keeping new users at Assisted. No forking, no "lite" version.

## What you'd do differently

Do this exercise again with real-life scenarios. Build Cortex to handle escalations on Sev-1 bugs, or to draft roadmap comms for embargoed launches, or to queue stories for a specific eng team. Each scenario has different blast radius, different stakeholders, different trust requirements. One abstract exercise taught me the pattern; real scenarios would teach me the judgment calls.
