# M1 Lab: Agent Line Map

**Cortex Workflow: Above/Below the Human Line**

---

## The Workflow (Decision by Decision)

| Action | Reversibility | Blast Radius | Measurability | Above/Below | HITL? |
|--------|---------------|--------------|----------------|-------------|-------|
| 1. Pull project state + activity | H | L | H | BELOW | No |
| 2. Decide relevant context | M | M | M | ABOVE | Yes |
| 3. Draft the update | H | L | H | BELOW | No |
| 4. Decide tone/commitment level | M | M | L | ABOVE | Yes |
| 5. Flag at-risk/escalation | H | L | H | BELOW | No |
| 6. Choose what to escalate | M | H | M | ABOVE | Yes |
| 7. Propose a story batch (capped) | H | M | H | ABOVE | Yes |
| 8. Post an update / approve a company-wide one | L | H | H | ABOVE | Yes |

---

## Per-Decision Justifications

**1. Pull project state + activity** sits BELOW the line because it's high reversibility (read-only), has low blast radius, and is high measurability to verify — deciding factor: reversibility.

**2. Decide relevant context** sits ABOVE the line because it's medium reversibility, has medium blast radius, and is medium measurability to verify — deciding factor: judgment call (context matters).

**3. Draft the update** sits BELOW the line because it's high reversibility, has low blast radius, and is high measurability to verify — deciding factor: measurability.

**4. Decide tone/commitment level** sits ABOVE the line because it's medium reversibility, has medium blast radius, and is low measurability to verify — deciding factor: measurability (tone is subjective).

**5. Flag at-risk/escalation** sits BELOW the line because it's high reversibility (rule-based), has low blast radius, and is high measurability to verify — deciding factor: reversibility.

**6. Choose what to escalate** sits ABOVE the line because it's medium reversibility, has high blast radius, and is medium measurability to verify — deciding factor: blast radius (wrong escalation causes real damage).

**7. Propose a story batch (capped)** sits ABOVE the line because it's high reversibility, has medium blast radius, and is high measurability to verify — deciding factor: blast radius (batch shape matters).

**8. Post an update / approve a company-wide one** sits ABOVE the line because it's low reversibility (can't unsend), has high blast radius, and is high measurability to verify — deciding factor: reversibility (irreversible).

---

## Agent Anatomy (Sketch)

- **Model:** Claude 3.5 Sonnet (default for speed + reasoning)
- **Escalation:** To Claude 3.5 Opus if draft requires company-wide tone judgment or high-stakes escalation reasoning
- **Tools:** Slack API (read project state, post updates), Jira API (fetch tickets), Calendar API (event context)
- **Memory:** Recent project state, escalation history, team context
- **Loop/bounds/evals:** Deferred to M2

---

## Hardest Call

**Action 3 (Draft the update)** was hardest to place — it could live above (judgment call on messaging) or below (just write it). Measurability was the deciding factor: because it's easy to verify if a draft is good or bad, Cortex can safely own it, but a human still reviews before send (HITL in Step 1 → BELOW in Step 3).

---

**Status:** ✅ Complete. All 8 actions scored, placed, and justified.
