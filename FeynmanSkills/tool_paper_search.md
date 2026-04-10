# TOOL: Paper Search & arXiv Access

## Purpose
This file defines the exact protocol for searching academic papers and fetching content from arXiv, substituting for Feynman's native AlphaXiv (`alpha` CLI) integration.

---

## arXiv Search Protocol

### Method 1: Web Search (primary)
Use the web_search tool with targeted queries:

```
arxiv.org <topic keywords>
arxiv <author name> <topic>
arxiv <year> <topic> <method>
```

Examples:
- `arxiv.org mechanistic interpretability transformers 2024`
- `arxiv scaling laws language models Hoffmann`
- `arxiv 2024 RLHF alternatives reward modeling`

Generate **2–4 varied queries** per topic to catch papers that use different terminology. Never repeat the same query twice.

### Method 2: Direct arXiv Fetch
If you have an arXiv ID (format: `YYMM.NNNNN` or `YYMM.NNNNN`):

- Abstract page: `https://arxiv.org/abs/<ID>`
- PDF (attempt): `https://arxiv.org/pdf/<ID>`
- HTML version (newer papers): `https://ar5iv.org/abs/<ID>` (rendered HTML, often more parseable)

### Method 3: Semantic Scholar / Papers With Code
For citation metadata and related-paper discovery:
- `https://www.semanticscholar.org/paper/<title-slug>`
- `https://paperswithcode.com/paper/<title-slug>`
- Web search: `semantic scholar "<paper title>"`

---

## What to Extract From Each Paper

When fetching an arXiv abstract page, extract:

| Field | Where to Find It |
|-------|----------------|
| Title | `<h1>` or title tag |
| Authors | Author list below title |
| Date submitted | Submission info |
| Abstract | Abstract section |
| Subjects / Categories | e.g., cs.LG, stat.ML |
| Comments | Conference/journal info if accepted |
| arXiv ID | From URL |
| Related links | Code, datasets, project pages linked in abstract/comments |

When fetching the full paper (PDF or HTML), additionally extract:
- Methodology (Section 3 or Methods)
- Main results / key numbers (Results section, main tables)
- Stated limitations (Limitations section)
- Key references the paper builds on

---

## AlphaXiv Capability Substitutions

| AlphaXiv Command | Claude Equivalent |
|-----------------|-------------------|
| `alpha search "scaling laws"` | `web_search("arxiv.org scaling laws language models")` + scan results |
| `alpha get 2401.12345` | Fetch `https://arxiv.org/abs/2401.12345` |
| `alpha ask 2401.12345 "What optimizer?"` | Fetch paper, then answer the specific question from its content |
| `alpha code https://github.com/org/repo src/model.py` | Fetch `https://raw.githubusercontent.com/org/repo/main/src/model.py` |
| `alpha annotate` | Not available — user maintains their own notes |

---

## Source Quality Evaluation

When prioritizing which papers to read in depth, use these signals:

**Higher priority:**
- Published at top venues (NeurIPS, ICML, ICLR, CVPR, ACL, EMNLP, ICCV, ECCV, SIGKDD, etc.)
- High citation count (visible on Semantic Scholar)
- Survey/review papers on the topic
- Papers from well-known research groups active in the area
- Papers that appear across multiple search angles

**Lower priority:**
- Workshop papers (still useful, but preliminary)
- Preprints with no peer review signal
- Papers with only self-citations in references
- Papers where the abstract makes very broad, unqualified claims

**Always note:**
- Preprint vs. peer-reviewed
- Conference vs. journal publication
- Submission date vs. publication date (some papers circulate for years as preprints)

---

## Citation Format Standards

### Inline citation:
`[Smith et al., 2024](https://arxiv.org/abs/2401.12345)`

### Reference list entry:
`[N] Smith, J., Doe, A., Johnson, B. "Title of the Paper." In *Proceedings of NeurIPS 2024*. https://arxiv.org/abs/2401.12345`

### For web sources:
`[N] Blog/Org Name. "Title of Post." *Publication Name*, Month Year. https://full-url`

### For GitHub repos:
`[N] Author/Org. "[repo-name]." GitHub, Year. https://github.com/org/repo`

---

## Handling Inaccessible Papers

When a paper is behind a paywall or the PDF is not fetchable:
1. Note it explicitly: `[PDF not accessible — abstract only]`
2. Use the abstract for high-level claims only
3. Search for: an arXiv preprint version, an open-access mirror, an author's personal/lab page
4. Do not fabricate content from the paper

---

## Domain-Specific Search Tips

**Machine Learning / AI:**
- Primary: arxiv.org (cs.LG, cs.AI, stat.ML)
- Secondary: Papers With Code (benchmark results), Hugging Face blog
- Conferences: NeurIPS, ICML, ICLR, CVPR, ACL, EMNLP

**Mathematics:**
- Primary: arxiv.org (math.*)
- Secondary: MathSciNet (abstracts only without subscription)

**Physics:**
- Primary: arxiv.org (physics.*, cond-mat.*, hep-*)
- Secondary: Inspire-HEP for particle physics

**Computer Science (systems, theory):**
- Primary: arxiv.org (cs.DS, cs.CC, cs.PL, cs.OS)
- Secondary: ACM Digital Library (abstracts), DBLP

**Biology / Bioinformatics:**
- Primary: bioRxiv (preprints), PubMed (peer-reviewed)
- Secondary: biorxiv.org search
