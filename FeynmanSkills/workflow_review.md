# WORKFLOW: /review

## Trigger
User types `/review <paper>` where `<paper>` is an arXiv ID, a URL, a pasted abstract/draft, or an uploaded document. Also triggered by: "peer review this", "give me feedback on this paper", "critique this draft".

## What This Workflow Produces
A structured, severity-graded simulated peer review covering: methodology, claims vs. evidence, experimental design, reproducibility, writing quality, and completeness. Formatted like an actual conference reviewer report.

---

## Execution Protocol

### PHASE 1 — RETRIEVE THE DOCUMENT

If given an arXiv ID (e.g., `2401.12345`):
1. Fetch `https://arxiv.org/abs/2401.12345` — get title, authors, abstract, metadata
2. Attempt to fetch `https://arxiv.org/pdf/2401.12345` if full content is needed
3. Note the venue/conference if listed

If given a URL: fetch the page directly.
If given pasted text or an uploaded file: use that content directly.

---

### PHASE 2 — REVIEWER AGENT

Read the document end-to-end and evaluate across all dimensions:

#### Evaluation Dimensions

**1. Claims vs. Evidence**
- Does the evidence presented actually support the conclusions?
- Are any conclusions stronger than the data warrants?
- Are confidence intervals, effect sizes, and statistical tests appropriate?

**2. Methodology**
- Is the approach sound? Well-motivated?
- Are there confounds or sources of bias not controlled for?
- Is the experimental setup described in enough detail to understand what was done?

**3. Experimental Design**
- Are baselines appropriate and competitive?
- Are ablation studies sufficient to isolate the contribution?
- Is the evaluation protocol fair? Are metrics appropriate for the claims?

**4. Reproducibility**
- Could someone replicate this from the description alone?
- Are hyperparameters, random seeds, dataset splits, and training details reported?
- Is code available? If so, does it appear to match the paper?

**5. Writing Quality**
- Is the paper clearly organized?
- Are claims stated precisely, not vaguely?
- Are figures and tables informative and labeled correctly?

**6. Completeness**
- Is related work adequately covered? Are important prior works missing?
- Are limitations discussed honestly?
- Is the contribution scoped appropriately?

---

### PHASE 3 — OUTPUT FORMAT

```
# Peer Review: [Paper Title]
**arXiv ID / URL:** [link]
**Authors:** [names]
**Venue (if known):** [venue]

---

## Summary Assessment
[2–4 sentence overall take: what does the paper do, does it succeed, what's the biggest issue?]

**Overall Recommendation:** Accept / Major Revision / Reject
**Confidence:** High / Medium / Low

---

## Strengths
- [Strength 1]
- [Strength 2]
- ...

---

## Critical Issues [CRITICAL]
> These are fundamental problems that undermine validity.

**[CRITICAL-1]** [Description of issue, with specific reference to section/claim]
- *Evidence:* [What in the paper triggers this concern]
- *Suggested fix:* [What would resolve it]

---

## Major Issues [MAJOR]
> Significant problems that should be addressed before acceptance.

**[MAJOR-1]** [Description]
- *Evidence:* ...
- *Suggested fix:* ...

---

## Minor Issues [MINOR]
> Improvements that would strengthen the paper.

**[MINOR-1]** [Description]

---

## Nits [NIT]
> Small stylistic or formatting issues.

- [NIT-1] ...

---

## Inline Annotations
[Specific comments tied to sections, figures, equations, or claims in the text]

- **Section 3.2:** [Comment]
- **Equation (4):** [Comment]
- **Table 2:** [Comment]

---

## Revision Plan
[Ordered list of what the authors should do, from most to least critical]
1. [Most critical fix]
2. ...
```

---

### PHASE 4 — VERIFIER AGENT

- Confirm that each identified issue is actually present in the paper (not a misread)
- Check confidence level on each finding
- Note if any findings require domain expertise beyond what's available (flag explicitly)

---

## Customization Notes
- User can focus the review: "focus only on the statistical methodology", "ignore the writing quality, focus on the science"
- The reviewer simulates the perspective of a knowledgeable peer in the field — not a hostile rejection, not a rubber stamp
- For user's own drafts: reviewer is constructive, actionable, honest
