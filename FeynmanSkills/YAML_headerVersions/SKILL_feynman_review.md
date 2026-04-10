---
name: feynman-review
description: Feynman peer review workflow. Activate when user types /review, or asks to "peer review this paper", "give me feedback on this draft", "critique this", "review arxiv:XXXXXXX", "what are the weaknesses of this paper", or pastes a paper abstract/draft and asks for evaluation. Produces severity-graded academic feedback: CRITICAL, MAJOR, MINOR, NIT.
---

# WORKFLOW: /review

## What This Workflow Produces
A structured, severity-graded simulated peer review covering: methodology, claims vs. evidence, experimental design, reproducibility, writing quality, and completeness. Formatted like an actual conference reviewer report.

---

## Execution Protocol

### PHASE 0 — RETRIEVE THE DOCUMENT

If given an arXiv ID (e.g., `2401.12345`):
1. Fetch `https://arxiv.org/abs/2401.12345` — title, authors, abstract, metadata
2. Attempt `https://arxiv.org/pdf/2401.12345` if full content needed
3. Note the venue/conference if listed

If given a URL: fetch directly.
If given pasted text or uploaded file: use that content directly.

---

### PHASE 1 — 🔬 REVIEWER AGENT (announce this phase)

#### Evaluation Dimensions

**1. Claims vs. Evidence**
- Does the evidence actually support the conclusions?
- Are conclusions stronger than the data warrants?
- Are statistical tests, effect sizes, confidence intervals appropriate?

**2. Methodology**
- Is the approach sound and well-motivated?
- Are there confounds or sources of bias not controlled for?
- Is the setup described in enough detail to understand what was done?

**3. Experimental Design**
- Are baselines appropriate and competitive?
- Are ablation studies sufficient to isolate the contribution?
- Is the evaluation protocol fair? Are metrics appropriate?

**4. Reproducibility**
- Could someone replicate this from the description alone?
- Are hyperparameters, seeds, dataset splits, training details reported?
- Is code available? Does it match the paper?

**5. Writing Quality**
- Is the paper clearly organized?
- Are claims stated precisely?
- Are figures and tables informative and correctly labeled?

**6. Completeness**
- Is related work adequately covered?
- Are limitations discussed honestly?
- Is the contribution scoped appropriately?

---

### PHASE 2 — OUTPUT FORMAT

```
# Peer Review: [Paper Title]
**arXiv / URL:** [link]
**Authors:** [names]
**Venue (if known):** [venue]

## Summary Assessment
[2–4 sentences: what does the paper do, does it succeed, biggest issue]

**Overall Recommendation:** Accept / Major Revision / Reject
**Confidence:** High / Medium / Low

## Strengths
- [Strength 1]
- [Strength 2]

## Critical Issues [CRITICAL]
> Fundamental problems that undermine validity.

**[CRITICAL-1]** [Description referencing specific section/claim]
- *Evidence:* [What triggers this concern]
- *Suggested fix:* [What would resolve it]

## Major Issues [MAJOR]
**[MAJOR-1]** [Description]
- *Evidence:* ...
- *Suggested fix:* ...

## Minor Issues [MINOR]
**[MINOR-1]** [Description]

## Nits [NIT]
- [NIT-1] ...

## Inline Annotations
- **Section 3.2:** [Comment]
- **Table 2:** [Comment]
- **Equation (4):** [Comment]

## Revision Plan
[Ordered list from most to least critical]
1. [Most critical fix]
2. ...
```

---

### PHASE 3 — ✅ VERIFIER AGENT (announce this phase)

- Confirm each identified issue is actually present (not a misread)
- Check confidence level on each finding
- Note if findings require domain expertise beyond what's available (flag explicitly)

---

## Customization
- User can focus: "only check the statistical methodology", "ignore writing, focus on the science"
- For user's own drafts: reviewer is constructive, actionable, honest — not hostile
