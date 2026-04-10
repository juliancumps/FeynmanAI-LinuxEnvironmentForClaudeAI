---
name: feynman-autoresearch-watch
description: Feynman autoresearch and watch workflows. Activate when user types /autoresearch or /watch, or asks for "iterative exploration", "optimize X through experimentation", "autonomous research loop", "keep tabs on X", "monitor new papers on Y", "check for recent developments in Z", or "what's new in X field". Autoresearch runs an iterative hypothesis-experiment-analysis loop. Watch does a fresh scan for new papers and developments on a topic.
---

# WORKFLOW: /autoresearch

## What This Workflow Produces
An iterative hypothesis → experiment design → analysis → decision loop. Since Claude cannot run actual experiments, this workflow produces: a structured research plan with iterative decision logic, a first-pass hypothesis test using available literature, and recommended next-iteration directions.

---

## Execution Protocol

### PHASE 1 — 🔍 RESEARCHER AGENT: Goal Analysis (announce this phase)

Parse the research idea into:
- **Optimization target**: What are we trying to maximize/minimize/discover?
- **Search space**: What variables/approaches to explore?
- **Feedback signal**: How do we know if a hypothesis worked?
- **Baseline**: What is the current best-known result?

Search for:
- Prior work attempting similar optimization
- Known pitfalls and failure modes in this search space
- Existing benchmarks or evaluation datasets

---

### PHASE 2 — ITERATIVE LOOP (simulate N=3 iterations, announce each)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ITERATION [N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Hypothesis:**
[What specific thing are we testing? Why might it work?]

**Experiment Design:**
[Exact setup: what to implement, what dataset/benchmark, what metric, what control]
```python
# Pseudocode or actual code for the experiment
```

**Predicted Result:**
[Based on theory and prior literature, what do we expect?]

**Literature Support:**
[Does prior work support this hypothesis? Cite sources.]

**Analysis (simulated):**
[Given the experiment design, what is the likely outcome based on related work?]
- If result > baseline: proceed with [variation]
- If result ≈ baseline: try [alternative]
- If result < baseline: abandon, pivot to [alternative]

**Decision:** Continue / Vary / Pivot — and why
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### PHASE 3 — ✍️ WRITER AGENT: Summary (announce this phase)

```
# Autoresearch Summary: [Goal]

## Experiment History
| Iteration | Hypothesis | Expected Result | Decision |
|-----------|-----------|-----------------|---------|
| 1 | [hypothesis] | [prediction] | Continue/Pivot |
| 2 | [hypothesis] | [prediction] | Continue/Pivot |
| 3 | [hypothesis] | [prediction] | Continue/Pivot |

## Best Configuration Found
[The approach most likely to work based on the iterative analysis]

## Ablation Summary
[Which factors matter most? What can be simplified?]

## Recommended Next Steps (for actual execution)
1. [Most promising experiment to actually run]
2. [Second priority]

## References
[All sources consulted]
```

---
---

# WORKFLOW: /watch

## What This Workflow Produces
A single fresh check of what's new on a topic — new papers, articles, releases. Plus a reusable watch specification the user can re-invoke anytime.

**Limitation:** The real Feynman CLI schedules recurring checks automatically. Claude cannot do this across sessions. This workflow runs a fresh check now and gives you a prompt to reuse.

---

## Execution Protocol

### PHASE 1 — 🔍 FRESH CHECK (announce this phase)

Search for recent material on the topic:
- arXiv search with date filter (last 30–60 days): web search `arxiv.org <topic> 2025` or `2026`
- Web search for recent blog posts, announcements, releases
- GitHub search for new relevant repos or significant updates

Compare against what's already known (scan conversation history for prior discussion).
Surface only **genuinely new** material — not things already discussed.

---

### PHASE 2 — 🔬 RELEVANCE FILTER (announce this phase)

For each new result:
- Is it genuinely relevant? (not just keyword-matched)
- Is it a significant contribution or incremental noise?
- Rate: `[HIGH]`, `[MEDIUM]`, `[LOW]`

---

### PHASE 3 — ✍️ WRITER AGENT: Watch Report (announce this phase)

```
# Watch Report: [Topic]
**Check date:** [today] | **Scope:** Last [N] days

## New Papers [HIGH]
**[Paper Title]** ([Author et al., YEAR](URL))
[2–3 sentence summary and why it's relevant]

## New Articles / Posts [HIGH]
**[Title]** ([Source, DATE](URL))
[Brief summary]

## Medium-relevance Items [MEDIUM]
- [Title](URL) — [one line]

## Summary
[1 paragraph: most important development this period? Has anything changed the landscape?]

## Watch Specification (re-run anytime)
To run this watch check again in a future session, paste:
> "Run /watch on [topic]. Check for papers and articles from the last 30 days.
> Prior findings summary: [brief summary of what's already known]."
```
