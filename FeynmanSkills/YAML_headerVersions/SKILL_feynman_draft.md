---
name: feynman-draft
description: Feynman draft writing workflow. Activate when user types /draft, or asks to "write a paper on X", "draft a report", "write this up as a document", "turn the findings into a paper", "write a survey of X", or uses /draft --from-session to write from prior research in this conversation. Produces academic-style documents with proper citations, structure, and narrative flow.
---

# WORKFLOW: /draft

## What This Workflow Produces
A structured academic-style document with proper citations, section structure, and narrative flow. Can be a full paper draft, research brief, survey section, technical report, or academic blog post.

## Key Distinction
- **After `/deepresearch` or `/lit` in same session**: Skip research phase, write directly from gathered findings
- **Fresh start**: Run Researcher pass first to gather material

---

## Execution Protocol

### PHASE 0 — CONTEXT CHECK

Scan conversation history: Has `/deepresearch` or `/lit` already run this session?
- **Yes** → Go directly to PHASE 2 (Writer). Use gathered findings.
- **No** → Run PHASE 1 (Researcher) first.

---

### PHASE 1 — 🔍 RESEARCHER AGENT (only if no prior context, announce this phase)

Follow same paper search protocol as /deepresearch:
- 3–4 angle searches on arXiv and web
- Extract and organize findings before writing

---

### PHASE 2 — ✍️ WRITER AGENT (announce this phase)

```markdown
# [Title]
**Date:** [today] | **Topic:** [topic]

## Abstract
[4–6 sentences: problem, why it matters, what's covered, key takeaway]

## 1. Introduction
[Motivation and context. What problem? Why now? Document structure overview.]

## 2. Background
[Prerequisites and prior work. What does the reader need to know?]
- Cite foundational papers with links

## 3. [Core Section(s)]
[Main content — adapt to topic]

### 3.1 [Subsection]
[Content with inline citations: [Author et al., YEAR](URL)]

## 4. Discussion
[Interpretation of findings. Implications. What should the field do next?]

## 5. Limitations
[Honest assessment of scope and gaps. What this does NOT cover and why.]

## 6. Conclusion
[Concise summary. No new information.]

## References
[1] Author, A. "Title." *Conference*, Year. https://arxiv.org/abs/ID
[2] ...

---
## [PLACEHOLDER — Human Input Needed]
> - [Section X]: Needs [specific information]
> - [Section Y]: Needs [specific experiment/data]
```

---

### PHASE 3 — 🔬 REVIEWER AGENT (self-review pass, announce this phase)

After writing:
- Does each section's claims match cited sources?
- Is the argument logically structured?
- Are any claims too strong for the evidence?
- Are there missing citations?
- Are PLACEHOLDER sections clearly marked?

Fix issues inline. Note unresolved concerns at the end.

---

## Document Type Adaptations

| Type | Structure |
|------|-----------|
| Research Brief | Summary, Key Findings, Open Questions, References |
| Literature Review | Scope, Consensus, Disagreements, Timeline, References |
| Technical Paper | Full IMRaD (Introduction, Methods, Results, Discussion) |
| Comparison Report | Source summaries, matrices, synthesis |

---

## Citation Rule
Never invent citations. If a claim needs one and none was found: write `[CITATION NEEDED]` and note it in Limitations.
