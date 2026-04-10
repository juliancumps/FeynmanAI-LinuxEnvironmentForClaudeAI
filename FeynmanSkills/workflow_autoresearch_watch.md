# WORKFLOW: /autoresearch

## Trigger
User types `/autoresearch <idea>` or asks for "iterative exploration", "optimize X", "run an experiment loop on Y", "autonomous research into Z".

## What This Workflow Produces
An iterative hypothesis → experiment design → analysis → decision loop. Since Claude cannot run actual experiments, this workflow produces: a structured research plan with iterative decision logic, a first-pass hypothesis test using available literature, and a recommended next-iteration direction based on results.

## Important Limitation
The real Feynman CLI can execute code, run training jobs, and loop autonomously. Claude cannot. Instead, this workflow simulates the **reasoning structure** of autoresearch: it runs the intellectual loop using literature and analysis rather than live experiments. It tells you what to run and what to expect.

---

## Execution Protocol

### PHASE 1 — RESEARCHER AGENT: Goal Analysis

Parse the research idea into:
- **The optimization target**: What are we trying to maximize/minimize/discover?
- **The search space**: What are the variables/approaches to explore?
- **The feedback signal**: How do we know if a hypothesis worked?
- **The baseline**: What is the current best-known result?

Search for:
- Prior work attempting similar optimization
- Known pitfalls and failure modes in this search space
- Existing benchmarks or evaluation datasets

---

### PHASE 2 — ITERATIVE LOOP (simulate N=3 iterations)

For each iteration, execute this loop and label it explicitly:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ITERATION [N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Hypothesis:**
[What specific thing are we testing this round? Why do we think it might work?]

**Experiment Design:**
[Exact setup: what to implement, what dataset/benchmark to evaluate on, what metric to track, what the control condition is]
```python
# Pseudocode or actual code for the experiment
```

**Predicted Result:**
[Based on theory and prior literature, what do we expect to happen?]

**Literature Support:**
[Does prior work support this hypothesis? Cite sources.]

**Analysis (simulated):**
[Given the experiment design, analyze what the likely outcome is based on related work]
- If result > baseline: proceed with [variation]
- If result ≈ baseline: try [alternative]
- If result < baseline: abandon direction, pivot to [alternative]

**Decision:**
[Continue / Vary / Pivot — and why]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### PHASE 3 — WRITER AGENT: Summary Report

```
# Autoresearch Summary: [Goal]

## Experiment History
| Iteration | Hypothesis | Expected Result | Decision |
|-----------|-----------|-----------------|---------|
| 1 | [hypothesis] | [prediction] | Continue/Pivot |
| 2 | [hypothesis] | [prediction] | Continue/Pivot |
| 3 | [hypothesis] | [prediction] | Continue/Pivot |

## Best Configuration Found (via literature analysis)
[Description of the approach most likely to work based on the iterative analysis]

## Ablation Summary
[Which factors matter most based on the analysis? What can be simplified?]

## Recommended Next Steps (for actual execution)
[Ordered list of what to actually run, based on the autoresearch findings]
1. [Most promising experiment to run first]
2. [Second priority]
3. ...

## References
[All sources consulted during the loop]
```

---

---

# WORKFLOW: /watch

## Trigger
User types `/watch <topic>` or asks to "keep tabs on X", "monitor new papers on Y", "check for recent developments in Z".

## What This Workflow Produces
A single fresh check of the current state of a topic — what's new since some prior point in time. Also produces a "watch specification" that the user can re-invoke manually whenever they want an update.

## Important Limitation
The real Feynman CLI schedules recurring background checks automatically. Claude cannot do this across sessions. Instead, this workflow: (1) runs a fresh check now, and (2) gives you a reusable prompt you can paste to run the check again anytime.

---

## Execution Protocol

### PHASE 1 — FRESH CHECK

1. Search for recent papers and articles on the topic:
   - arXiv search with date filter (last 30–60 days)
   - Web search for recent blog posts, announcements, releases
   - GitHub search for new relevant repos or significant updates

2. Compare against what's already known (scan conversation history for prior discussion of this topic)

3. Surface only **genuinely new** material — not things already discussed

### PHASE 2 — RESEARCHER AGENT: Relevance Filter

For each new result found:
- Is it genuinely relevant to the stated watch topic? (not just keyword-matched)
- Is it a significant contribution or just incremental noise?
- Rate relevance: `[HIGH]`, `[MEDIUM]`, `[LOW]`

### PHASE 3 — WRITER AGENT: Watch Report

```
# Watch Report: [Topic]
**Check date:** [today]
**Scope:** Last [N] days

---

## New Papers [HIGH relevance]
**[Paper Title]** ([Author et al., YEAR](URL))
[2–3 sentence summary of contribution and why it's relevant to the watch topic]

**[Paper Title]** ...

---

## New Articles / Posts [HIGH relevance]
**[Title]** ([Source, DATE](URL))
[Brief summary]

---

## Medium-relevance Items [MEDIUM]
- [Title](URL) — [one line]
- ...

---

## Low-signal / Noise [LOW]
[Skipped or listed briefly]

---

## Summary
[1 paragraph: what's the most important development in this watch period? Has anything changed the landscape?]

---

## Watch Specification (re-run anytime)
To run this watch check again in a future session, paste:
> "Run /watch on [topic]. Check for papers and articles from the last 30 days. 
> Compare against prior findings: [brief summary of what's already known]."
```
