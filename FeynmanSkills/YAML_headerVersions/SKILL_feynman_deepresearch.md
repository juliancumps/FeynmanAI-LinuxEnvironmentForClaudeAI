---
name: feynman-deepresearch
description: Feynman deep research workflow. Activate when user types /deepresearch, or asks for a "deep dive", "thorough investigation", "comprehensive research", "full breakdown", or "everything you can find" on a topic. Runs multi-angle Researcher → Writer → Verifier agent pipeline and produces a fully cited research brief.
---

# WORKFLOW: /deepresearch

## What This Workflow Produces
A structured research brief with: Summary, Background, Key Findings (by theme), Open Questions, and a full References section. Every claim is cited. Every source URL is real and verified.

---

## Execution Protocol

### PHASE 1 — 🔍 RESEARCHER AGENT (announce this phase)

**Step 1: Query decomposition**
Break the topic into 3–4 sub-angles. Example for "mechanistic interpretability in LLMs":
- Angle A: Foundational theory and definitions
- Angle B: Recent empirical results and methods (last 2 years)
- Angle C: Competing approaches and critiques
- Angle D: Engineering implementations and tools

**Step 2: Search execution**
For each angle:
1. Run a web search targeting `arxiv.org` OR general web search
2. Fetch the abstract page for top 2–3 arXiv results: `https://arxiv.org/abs/<ID>`
3. Search for blog posts, documentation, repos on non-academic angles
4. Record for each source:
   - Title, Authors, Year, arXiv ID or URL
   - Key claims extracted
   - Methodology summary
   - Results summary
   - Stated limitations
   - Links to related work cited

**Step 3: Source prioritization**
Rank by: publication venue quality, citation count (if visible), recency, topical relevance. Note when a source is a preprint vs peer-reviewed.

---

### PHASE 2 — ✍️ WRITER AGENT (announce this phase)

Synthesize all gathered material into this structure:

```
# [Topic] — Research Brief

## Summary
[3–5 sentence overview of the landscape and key takeaways]

## Background
[Context, motivation, why this topic matters]

## Key Findings
### [Theme 1]
[Findings with inline citations: [Author et al., YEAR](URL)]
### [Theme 2]
...

## Consensus and Disagreements
[Where does the field agree? Where are active debates?]

## Open Questions
[Unresolved issues, gaps, promising directions]

## References
- [1] Author et al., "Title", Year. https://arxiv.org/abs/ID
- [2] ...
```

---

### PHASE 3 — ✅ VERIFIER AGENT (announce this phase)

For each major claim in Key Findings:
1. Re-check: does the cited source actually support this claim?
2. Classify: `[VERIFIED]`, `[OVERSTATED]`, `[UNSUPPORTED]`, `[CONTRADICTED]`
3. Flag any dead links or inaccessible sources
4. Correct or caveat any failed verifications inline

---

## Customization
- User can constrain: "focus on papers from 2023–2025", "only empirical results", "ignore survey papers"
- Narrow topics → focused brief; broad topics → survey-style overview
- If little arXiv coverage exists, lean on web search + GitHub + documentation
