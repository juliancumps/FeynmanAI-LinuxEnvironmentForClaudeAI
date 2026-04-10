---
name: feynman-paper-search
description: Feynman paper search and arXiv access protocol. Load this whenever searching for academic papers, fetching arXiv content, looking up citations, checking paper metadata, finding GitHub repos associated with papers, or evaluating source quality. This defines the exact search protocol, AlphaXiv substitutions, citation formatting standards, and domain-specific search tips that all other Feynman workflows depend on.
---

# TOOL: Paper Search & arXiv Access

## arXiv Search Protocol

### Method 1: Web Search (primary)
Use web_search with targeted queries:
```
arxiv.org <topic keywords>
arxiv <author name> <topic>
arxiv <year> <topic> <method>
```

Generate **2–4 varied queries** per topic to catch papers using different terminology. Never repeat the same query twice.

### Method 2: Direct arXiv Fetch
If you have an arXiv ID (format: `YYMM.NNNNN`):
- Abstract page: `https://arxiv.org/abs/<ID>`
- PDF (attempt): `https://arxiv.org/pdf/<ID>`
- HTML version (more parseable): `https://ar5iv.org/abs/<ID>`

### Method 3: Semantic Scholar / Papers With Code
- Web search: `semantic scholar "<paper title>"`
- `https://paperswithcode.com/paper/<title-slug>`
- For citation metadata and related-paper discovery

---

## What to Extract From Each Paper

| Field | Where to Find |
|-------|--------------|
| Title | Title tag / h1 |
| Authors | Author list |
| Date submitted | Submission info |
| Abstract | Abstract section |
| Subject categories | e.g., cs.LG, stat.ML |
| Conference/journal | Comments field |
| arXiv ID | From URL |
| Code/project links | Abstract or comments |

From full paper (if accessible):
- Methodology (Section 3 / Methods)
- Main results and key numbers (Results, main tables)
- Stated limitations (Limitations section)
- Key references the paper builds on

---

## AlphaXiv Capability Substitutions

| AlphaXiv Command | Claude Equivalent |
|-----------------|-------------------|
| `alpha search "scaling laws"` | `web_search("arxiv.org scaling laws language models")` + scan results |
| `alpha get 2401.12345` | Fetch `https://arxiv.org/abs/2401.12345` |
| `alpha ask 2401.12345 "What optimizer?"` | Fetch paper, answer from its content |
| `alpha code https://github.com/org/repo src/model.py` | Fetch `https://raw.githubusercontent.com/org/repo/main/src/model.py` |
| `alpha annotate` | Not available — user maintains their own notes |

---

## Source Quality Evaluation

**Higher priority:**
- Published at top venues: NeurIPS, ICML, ICLR, CVPR, ACL, EMNLP, ICCV, SIGKDD
- High citation count (visible on Semantic Scholar)
- Survey/review papers on the topic
- Papers that appear across multiple search angles

**Lower priority:**
- Workshop papers (preliminary, useful but less authoritative)
- Preprints with no peer review signal
- Papers with only self-citations

**Always note:**
- Preprint vs. peer-reviewed
- Conference vs. journal
- Submission date vs. publication date

---

## Citation Format Standards

**Inline:**
`[Smith et al., 2024](https://arxiv.org/abs/2401.12345)`

**Reference list:**
`[N] Smith, J., Doe, A. "Title of the Paper." In *NeurIPS 2024*. https://arxiv.org/abs/2401.12345`

**Web sources:**
`[N] Blog/Org Name. "Title." *Publication*, Month Year. https://full-url`

**GitHub repos:**
`[N] Author/Org. "repo-name." GitHub, Year. https://github.com/org/repo`

---

## Handling Inaccessible Papers

When a paper is behind a paywall or PDF not fetchable:
1. Note explicitly: `[PDF not accessible — abstract only]`
2. Use abstract for high-level claims only
3. Search for: arXiv preprint version, open-access mirror, author's personal/lab page
4. Never fabricate content from the paper

---

## Domain-Specific Search Tips

| Domain | Primary | Secondary |
|--------|---------|-----------|
| ML / AI | arxiv.org (cs.LG, cs.AI, stat.ML) | Papers With Code, HuggingFace blog |
| Mathematics | arxiv.org (math.*) | MathSciNet (abstracts) |
| Physics | arxiv.org (physics.*, hep-*, cond-mat.*) | Inspire-HEP |
| CS Theory/Systems | arxiv.org (cs.DS, cs.CC, cs.PL) | ACM DL (abstracts), DBLP |
| Biology / Bioinformatics | bioRxiv, PubMed | biorxiv.org |
