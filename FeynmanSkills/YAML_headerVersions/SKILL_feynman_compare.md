---
name: feynman-compare
description: Feynman source comparison workflow. Activate when user types /compare, or asks to "compare these papers", "which approach is better", "contrast X and Y", "what do these papers agree on", "build a comparison matrix", or provides multiple arXiv IDs and asks for a side-by-side analysis. Produces a structured agreement/disagreement matrix with methodology differences and synthesis.
---

# WORKFLOW: /compare

## What This Workflow Produces
A structured comparison matrix showing where sources agree, disagree, and differ in methodology. Useful for understanding contradictory results, evaluating competing approaches, and writing Related Work sections.

---

## Execution Protocol

### PHASE 1 — 🔍 RESEARCHER AGENT (announce this phase)

**If a topic is given:** Find 3–5 most relevant and contrasting papers via arXiv + web search.
**If specific papers/IDs are given:** Fetch each one directly via `https://arxiv.org/abs/<ID>`.

For **each source independently**, extract:
- Core contribution / main claim
- Methodology (approach, model, algorithm)
- Evaluation (datasets, metrics, baselines)
- Key results (the numbers that matter)
- Stated limitations
- Implicit assumptions

Extract independently per source — do not let knowledge of one paper color extraction of another.

---

### PHASE 2 — ✍️ WRITER AGENT (announce this phase)

Align claims across sources:
- **Agreement**: Two+ papers make the same claim or reach compatible conclusions
- **Disagreement**: Papers report contradictory results or opposing claims
- **Non-overlapping**: Papers measure different things — note methodological differences that explain apparent contradictions

```
# Source Comparison: [Topic / Papers]

## Source Summaries
**[Paper 1]** ([Author et al., YEAR](URL))
[One paragraph: what it does, what it claims, how it evaluates]

**[Paper 2]** ...

## Agreement Matrix
| Claim | Paper 1 | Paper 2 | Paper N | Confidence |
|-------|---------|---------|---------|-----------|
| [Claim A] | ✓ | ✓ | ✓ | High |
| [Claim B] | ✓ | ✗ | — | Medium |

## Detailed Agreements
**[Claim A]:** [Statement of agreed claim]
- Paper 1: [How they support it] ([citation](URL))
- Paper 2: [How they support it] ([citation](URL))
- Assessment: Well-established across independent experiments

## Detailed Disagreements
**[Claim B]:** [Statement of contested claim]
- Paper 1 argues: [Position X] — [reason/evidence] ([citation](URL))
- Paper 2 argues: [Position Y] — [reason/evidence] ([citation](URL))
- Analysis: Genuine contradiction, or explained by [methodological difference]?
- Recommendation: [Which claim is better supported? What experiment would settle it?]

## Methodology Differences
| Dimension | Paper 1 | Paper 2 | Paper N |
|-----------|---------|---------|---------|
| Model size | [X]B params | [Y]B | [Z]B |
| Dataset | [name] | [name] | [name] |
| Metric | [metric] | [metric] | [metric] |
| Key baseline | [name] | [name] | [name] |

## Synthesis
### Well-supported claims (believe these):
- [Claim]: supported by N papers with consistent results

### Contested claims (proceed with caution):
- [Claim]: Papers X and Y disagree; likely depends on [factor]

### Open empirical questions:
- [Claim]: untested or results inconclusive

## References
[Full bibliography for all compared sources]
```

---

### PHASE 3 — ✅ VERIFIER AGENT (announce this phase)

- Verify that agreement/disagreement characterizations are accurate
- Check: are apparent disagreements actually methodological artifacts?
- Flag: if a paper is cited as supporting a claim but only partially does so

---

## Customization
- User can specify specific comparison dimensions: "only compare on benchmark X results"
- User can provide their own papers/abstracts to compare against the literature
- For Related Work sections: this workflow provides raw material; `/draft` structures it into prose
