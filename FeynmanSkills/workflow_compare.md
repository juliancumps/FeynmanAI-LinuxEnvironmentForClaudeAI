# WORKFLOW: /compare

## Trigger
User types `/compare <topic>` or `/compare <arXiv ID 1> <arXiv ID 2> ...`, or asks to "compare these papers", "which approach is better", "contrast X and Y", "what do these papers agree/disagree on".

## What This Workflow Produces
A structured comparison matrix showing where sources agree, disagree, and differ in methodology. Useful for understanding contradictory results, evaluating competing approaches, and writing Related Work sections.

---

## Execution Protocol

### PHASE 1 — RESEARCHER AGENT

**If a topic is given:** Find 3–5 most relevant and contrasting papers on the topic using the paper search protocol (arXiv + web).

**If specific papers/IDs are given:** Fetch each one directly.

For **each source independently**, extract:
- Core contribution / main claim
- Methodology (approach, model, algorithm)
- Evaluation (datasets, metrics, baselines)
- Key results (the numbers that matter)
- Stated limitations
- Implicit assumptions

Do this independently per source before comparing — do not let knowledge of one paper color the extraction of another.

---

### PHASE 2 — WRITER AGENT (Comparison Engine)

Align claims across sources:
- **Agreement**: Two or more papers make the same claim or reach compatible conclusions
- **Disagreement**: Papers report contradictory results or make opposing claims
- **Non-overlapping**: Papers measure different things and cannot be directly compared (note methodological differences that explain apparent contradictions)

```
# Source Comparison: [Topic / Papers]

---

## Source Summaries
**[Paper 1]** ([Author et al., YEAR](URL))
[One paragraph: what it does, what it claims, how it evaluates]

**[Paper 2]** ([Author et al., YEAR](URL))
[One paragraph: ...]

**[Paper N]** ...

---

## Agreement Matrix
| Claim | Paper 1 | Paper 2 | Paper N | Confidence |
|-------|---------|---------|---------|-----------|
| [Claim A] | ✓ Agrees | ✓ Agrees | ✓ Agrees | High |
| [Claim B] | ✓ Agrees | ✗ Disagrees | — Not tested | Medium |
| [Claim C] | — Not tested | ✓ Agrees | ✓ Agrees | Medium |

---

## Detailed Agreements
**[Claim A]: [Statement of the agreed-upon claim]**
- Paper 1: [How they support it] ([citation](URL))
- Paper 2: [How they support it] ([citation](URL))
- Assessment: Well-established; supported by multiple independent experiments

---

## Detailed Disagreements
**[Claim B]: [Statement of the contested claim]**
- Paper 1 argues: [Position X] — because [reason/evidence] ([citation](URL))
- Paper 2 argues: [Position Y] — because [reason/evidence] ([citation](URL))
- Analysis: Is this a genuine contradiction, or explained by [methodological difference]? [Assessment]
- Recommendation: [Which claim is better supported? What experiment would settle it?]

---

## Methodology Differences
| Dimension | Paper 1 | Paper 2 | Paper N |
|-----------|---------|---------|---------|
| Model size | [X]B params | [Y]B params | [Z]B params |
| Dataset | [name] | [name] | [name] |
| Evaluation metric | [metric] | [metric] | [metric] |
| Training compute | [FLOPs/hours] | [FLOPs/hours] | [FLOPs/hours] |
| Key baseline | [name] | [name] | [name] |

---

## Synthesis
[Overall assessment: Which claims have broad support? Which are contested? Which apparent disagreements are actually methodology artifacts?]

### Well-supported claims (believe these):
- [Claim]: supported by [N] papers with consistent results

### Contested claims (proceed with caution):
- [Claim]: Papers X and Y disagree; likely depends on [factor]

### Open empirical questions (nobody knows yet):
- [Claim]: untested or results inconclusive

---

## References
[Full bibliography for all compared sources]
```

---

### PHASE 3 — VERIFIER AGENT

- Verify that agreement/disagreement characterizations are accurate
- Check: are apparent disagreements actually explained by methodological differences?
- Flag: if a paper is frequently cited as supporting a claim but actually only partially supports it

---

## Customization Notes
- User can request specific dimensions to compare: "only compare on benchmark X results"
- User can provide their own papers (uploaded files or pasted abstracts) to compare against the literature
- For Related Work writing: this workflow provides the raw material; `/draft` can then structure it into prose
