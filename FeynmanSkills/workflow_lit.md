# WORKFLOW: /lit

## Trigger
User types `/lit <topic>` or requests a "literature review", "survey of the field", "what has been done on X", "related work on X".

## What This Workflow Produces
A structured literature review mapping: the state of the field, consensus positions, active debates, open questions, and a chronological timeline of key milestones. Primarily sourced from academic papers — this is not a general web research task.

## When to Use vs /deepresearch
- Use `/lit` when the goal is: *what does the field look like?* (landscape mapping)
- Use `/deepresearch` when the goal is: *what is the answer to this specific question?* (deep dive)
- `/lit` is particularly useful at the start of a research project or when writing a Related Work section

---

## Execution Protocol

### PHASE 1 — RESEARCHER AGENT

**Step 1: Paper discovery strategy**
Search with priority order:
1. Survey papers and reviews on the topic (search "survey", "review", "overview" + topic)
2. Foundational / most-cited papers (seminal work)
3. Recent papers (last 1–2 years) representing the current frontier
4. Contrarian or challenge papers that push back on mainstream views

Search strategy:
- `arxiv.org` searches using the `web_search` tool with domain filtering
- Fetch abstract pages: `https://arxiv.org/abs/<ID>`
- Cross-reference: look at "cited by" relationships when visible; use Semantic Scholar or Google Scholar links when available

**Step 2: Per-paper extraction**
For each paper, extract:
- Main claims and contributions
- Methodology (what did they do?)
- Results (what did they find?)
- Limitations explicitly stated
- How it relates to other papers (agrees with, contradicts, extends)
- Publication year, venue, arXiv ID

**Step 3: Field mapping**
Organize extracted material into:
- Claims where multiple papers agree (consensus)
- Claims where papers conflict (active debate)
- Topics the literature has not yet adequately addressed (gaps)
- Chronological milestones

---

### PHASE 2 — WRITER AGENT

Output format:

```
# Literature Review: [Topic]

## Scope and Methodology
[What was searched, what inclusion criteria were used, how many papers reviewed]

## Consensus
[Claims supported by multiple papers — cite all supporting sources inline]
- Finding 1: [explanation] ([Author, YEAR](URL); [Author, YEAR](URL))
- Finding 2: ...

## Active Disagreements
[Where papers contradict each other — characterize the debate fairly]
- Debate 1: [Paper A] argues X ([URL]), while [Paper B] argues Y ([URL]) because...
- Debate 2: ...

## Open Questions
[What the literature has not answered — specific and tractable where possible]
- Question 1: ...
- Question 2: ...

## Timeline of Key Milestones
| Year | Paper | Contribution |
|------|-------|-------------|
| YEAR | [Title](URL) | What it introduced/established |
| ...  | ...   | ... |

## References
[Full bibliography]
```

---

### PHASE 3 — VERIFIER AGENT

- Verify consensus claims are actually supported by the cited sources
- Verify that characterizations of debates are fair to both sides
- Flag any claims where only a single source exists (note fragility)
- Check for papers that contradict stated consensus that may have been missed

---

## Customization Notes
- User can specify: subfield focus, date range, specific authors/groups, venue type (NeurIPS, ICML, ICLR, etc.)
- For fast-moving fields: explicitly note when recent preprints may not yet be peer-reviewed
- Citation counts and publication venues should be noted as signals, but not treated as ground truth
