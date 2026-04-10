---
name: feynman-lit
description: Feynman literature review workflow. Activate when user types /lit, or asks for a "literature review", "survey of the field", "what has been done on X", "related work on X", "map the research landscape", or "what does the field say about X". Focused on academic paper discovery and field mapping — consensus, disagreements, open questions, timeline.
---

# WORKFLOW: /lit

## What This Workflow Produces
A structured literature review mapping: the state of the field, consensus positions, active debates, open questions, and a chronological timeline of key milestones. Primarily sourced from academic papers.

## When to Use vs /deepresearch
- `/lit` → *what does the field look like?* (landscape mapping)
- `/deepresearch` → *what is the answer to this specific question?* (deep dive)

---

## Execution Protocol

### PHASE 1 — 🔍 RESEARCHER AGENT (announce this phase)

**Paper discovery priority order:**
1. Survey papers and reviews ("survey", "review", "overview" + topic)
2. Foundational / most-cited papers (seminal work)
3. Recent papers (last 1–2 years) — current frontier
4. Contrarian or challenge papers pushing back on mainstream views

**Search strategy:**
- Web search targeting `arxiv.org` with domain filtering
- Fetch abstract pages: `https://arxiv.org/abs/<ID>`
- Cross-reference: Semantic Scholar, Google Scholar, Papers With Code

**Per-paper extraction:**
- Main claims and contributions
- Methodology
- Results
- Limitations
- How it relates to other papers (agrees, contradicts, extends)
- Publication year, venue, arXiv ID

**Field mapping:**
Organize extracted material into: consensus, active debate, gaps/open questions, chronological milestones

---

### PHASE 2 — ✍️ WRITER AGENT (announce this phase)

```
# Literature Review: [Topic]

## Scope and Methodology
[What was searched, inclusion criteria, how many papers reviewed]

## Consensus
[Claims supported by multiple papers — cite all supporting sources]
- Finding 1: [explanation] ([Author, YEAR](URL); [Author, YEAR](URL))

## Active Disagreements
[Where papers contradict each other]
- Debate 1: [Paper A] argues X ([URL]), while [Paper B] argues Y ([URL]) because...

## Open Questions
[What the literature has not answered]
- Question 1: ...

## Timeline of Key Milestones
| Year | Paper | Contribution |
|------|-------|-------------|
| YEAR | [Title](URL) | What it introduced |

## References
[Full bibliography]
```

---

### PHASE 3 — ✅ VERIFIER AGENT (announce this phase)

- Verify consensus claims are actually supported by cited sources
- Verify that characterizations of debates are fair to both sides
- Flag claims where only a single source exists (fragile consensus)
- Check for papers that contradict stated consensus that may have been missed

---

## Customization
- User can specify: subfield focus, date range, specific authors/groups, venue type
- For fast-moving fields: note when recent preprints may not be peer-reviewed
- Citation counts and venue are signals, not ground truth
