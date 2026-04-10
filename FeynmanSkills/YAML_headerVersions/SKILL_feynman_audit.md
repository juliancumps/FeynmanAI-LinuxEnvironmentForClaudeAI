---
name: feynman-audit
description: Feynman code audit workflow. Activate when user types /audit, or asks to "check if the code matches the paper", "audit this paper's codebase", "does the implementation match the claims", "reproducibility audit", or provides an arXiv ID and asks about code consistency. Compares paper claims against the actual codebase and flags mismatches, missing implementations, and reproducibility risks.
---

# WORKFLOW: /audit

## What This Workflow Produces
A structured audit report comparing a paper's stated claims against the actual codebase. Flags mismatches, missing implementations, and reproducibility risks with exact file references where possible.

---

## Execution Protocol

### PHASE 1 — LOCATE RESOURCES (announce this phase)

**Step 1: Get the paper**
- Fetch `https://arxiv.org/abs/<ID>`
- Extract all concrete claims: hyperparameters, architecture, training schedule, dataset prep, evaluation protocol, reported metrics/numbers

**Step 2: Find the codebase**
Priority order:
1. Paper itself — look for GitHub link or "Code available at..."
2. Papers With Code: search `paperswithcode.com` for the paper title
3. Web search: `"<paper title>" github site:github.com`
4. GitHub search: `<first author> <key method name>`

If no code found: note explicitly — absence of code is itself a reproducibility risk.

---

### PHASE 2 — 🔍 RESEARCHER AGENT: Paper Pass (announce this phase)

Extract all auditable claims into a structured list:

```
CLAIM LIST:
[C1] Architecture: [description] (Section X)
[C2] Optimizer: [name, lr, weight decay] (Section X)
[C3] Training: [epochs, batch size, hardware] (Section X)
[C4] Dataset: [name, splits, preprocessing] (Section X)
[C5] Evaluation: [metric, protocol, test set] (Section X)
[C6] Result: [reported number for benchmark Y] (Table Z)
...
```

---

### PHASE 3 — ✅ VERIFIER AGENT: Code Pass (announce this phase)

For each claim, locate corresponding implementation:
- Fetch key files: `config/`, `train.py`, `model.py`, `eval.py`, `README.md`
- Use `https://raw.githubusercontent.com/<org>/<repo>/main/<filepath>` for direct access
- Compare paper claim to code behavior

Classify each claim:
- `[MATCH]` — Code matches paper
- `[MISMATCH]` — Code differs; document both values
- `[MISSING]` — Claim in paper has no implementation
- `[UNDERDETERMINED]` — Cannot verify; code or claim too ambiguous

Also check reproducibility risks:
- Missing random seeds
- Non-deterministic operations without pinned versions
- Hardcoded paths
- No environment specification (`requirements.txt`, `conda.yaml`)
- Unpinned dependency versions

---

### PHASE 4 — ✍️ WRITER AGENT: Report (announce this phase)

```
# Code Audit Report: [Paper Title]
**arXiv:** [URL] | **Repo:** [URL]
**Audit Date:** [today]

## Match Summary
- Claims audited: N
- Matched: X (XX%)
- Mismatches: Y
- Missing implementations: Z
- Reproducibility risks: W

## Confirmed Claims [MATCH]
| Claim | Paper Says | Code Shows | File/Line |
|-------|-----------|------------|-----------|
| [C1]  | ...        | ...        | train.py:42 |

## Mismatches [MISMATCH]
**[C2] Optimizer learning rate**
- Paper claims: `lr=1e-4`
- Code uses: `lr=3e-4` (config/default.yaml:17)
- Impact: High — affects convergence

## Missing Implementations [MISSING]
**[C5] Evaluation protocol**
- Paper describes: [description]
- Code: No corresponding evaluation script found

## Reproducibility Risks
- [ ] No random seed set (train.py)
- [ ] PyTorch version unpinned in requirements.txt
- [ ] Hardcoded dataset path (dataloader.py:88)

## Verdict
[Overall assessment: Is this paper reproducible? What are the biggest blockers? Would you build on this?]
```

---

## Customization
- User can provide repo URL directly if they already have it
- If paper has multiple codebases, audit the official one first
- Also useful for auditing your own paper before submission
