# WORKFLOW: /audit

## Trigger
User types `/audit <arXiv ID>` or `/audit <repo URL> --paper <arXiv ID>`, or requests a "paper vs code audit", "check if the code matches the paper", "reproducibility audit".

## What This Workflow Produces
A structured audit report comparing a paper's stated claims (hyperparameters, architecture, training procedure, evaluation metrics, reported results) against the actual codebase. Flags mismatches, missing implementations, and reproducibility risks with exact file/line references where possible.

## Why This Matters
A surprisingly common problem in ML research: the paper describes one thing, the code does another. This workflow surfaces those gaps before you waste time building on faulty results.

---

## Execution Protocol

### PHASE 1 — LOCATE RESOURCES

**Step 1: Get the paper**
- Fetch `https://arxiv.org/abs/<ID>` for metadata and abstract
- Fetch full paper if accessible
- Extract all concrete claims: hyperparameters, architecture details, training schedule, dataset preparation, evaluation protocol, reported metrics and numbers

**Step 2: Find the codebase**
Priority order:
1. Check the paper itself for a GitHub link or "Code available at..." statement
2. Search Papers With Code: `https://paperswithcode.com/paper/<title-slug>` — fetch this page
3. Web search: `"<paper title>" github site:github.com`
4. Search GitHub directly: `<first author name> <key method name>`

If no code is found: note this explicitly — absence of code is itself a reproducibility risk.

---

### PHASE 2 — RESEARCHER AGENT (Paper Pass)

Extract all auditable claims from the paper and organize into a structured claim list:

```
CLAIM LIST:
[C1] Architecture: [description] (Section X, Page Y)
[C2] Optimizer: [name, lr, weight decay, etc.] (Section X)
[C3] Training: [epochs, batch size, hardware, duration] (Section X)
[C4] Dataset: [name, splits, preprocessing] (Section X)
[C5] Evaluation: [metric, protocol, test set] (Section X)
[C6] Result: [reported number for benchmark Y] (Table Z)
...
```

---

### PHASE 3 — VERIFIER AGENT (Code Pass)

For each claim, attempt to locate the corresponding implementation:
- Fetch key files from the repo: `config/`, `train.py`, `model.py`, `eval.py`, `README.md`
- Use `https://raw.githubusercontent.com/<org>/<repo>/main/<filepath>` for direct file access
- Compare paper claim to code behavior

For each claim, classify:
- `[MATCH]` — Code matches the paper description
- `[MISMATCH]` — Code differs from the paper; document both values
- `[MISSING]` — Claim in paper has no corresponding implementation
- `[UNDERDETERMINED]` — Can't verify; code is ambiguous or claim is too vague

Also check for reproducibility risks:
- Missing random seeds
- Non-deterministic operations without fixed versions
- Hardcoded paths
- Absent environment specification (no `requirements.txt`, no `conda.yaml`)
- Unpinned dependency versions

---

### PHASE 4 — WRITER AGENT (Report)

```
# Code Audit Report: [Paper Title]
**arXiv:** [URL] | **Repo:** [URL]
**Audit Date:** [today]

---

## Match Summary
- Claims audited: N
- Matched: X (XX%)
- Mismatches: Y
- Missing implementations: Z
- Reproducibility risks: W

---

## Confirmed Claims [MATCH]
| Claim | Paper Says | Code Shows | File/Line |
|-------|-----------|------------|-----------|
| [C1]  | ...        | ...        | train.py:42 |
| ...   | ...        | ...        | ... |

---

## Mismatches [MISMATCH]
**[C2] Optimizer learning rate**
- Paper claims: `lr=1e-4`
- Code uses: `lr=3e-4` (config/default.yaml, line 17)
- Impact: [High/Medium/Low] — affects convergence behavior

**[CX] ...**
...

---

## Missing Implementations [MISSING]
**[C5] Evaluation protocol**
- Paper describes: [description]
- Code: No corresponding evaluation script found
- Impact: Results cannot be reproduced without this

---

## Reproducibility Risks
- [ ] No random seed set in training script (train.py)
- [ ] PyTorch version unpinned in requirements.txt
- [ ] Hardcoded dataset path in dataloader.py:88
- [ ] No documentation of hardware used for reported results

---

## Verdict
[Overall assessment: Is this paper reproducible? What are the biggest blockers? Would you build on this?]
```

---

## Customization Notes
- User can provide the repo URL directly if they already have it
- If paper has multiple codebases (official + reproductions), audit the official one first
- This workflow is also useful for auditing your own paper before submission
