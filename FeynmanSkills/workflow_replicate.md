# WORKFLOW: /replicate

## Trigger
User types `/replicate <arXiv ID>` or `/replicate "<specific claim>"`, or asks to "plan a replication", "reproduce this result", "how would I implement this".

## What This Workflow Produces
A detailed, actionable replication plan: step-by-step instructions from environment setup through final evaluation, with hardware requirements, identified underspecifications, risk assessment, and success criteria. Claude cannot run experiments, but the plan should be detailed enough that the user can execute it.

---

## Execution Protocol

### PHASE 1 — RESEARCHER AGENT (Paper + Code Extraction)

**Step 1: Get everything**
- Fetch the paper: `https://arxiv.org/abs/<ID>`
- Find and fetch the codebase (same strategy as `/audit`)
- Extract every detail needed for reproduction:

```
REPRODUCTION CHECKLIST:
□ Model architecture (exact layers, dims, activations, normalization)
□ Optimizer (name, lr, schedule, warmup, weight decay, gradient clipping)
□ Training duration (steps/epochs, batch size, gradient accumulation)
□ Dataset (name, version, splits, preprocessing, augmentation)
□ Evaluation protocol (metric, test set, averaging strategy)
□ Hardware (GPU type, count, training time reported)
□ Random seeds
□ Mixed precision / bf16 / fp16 settings
□ Key hyperparameters (temperature, dropout, etc.)
□ Environment (framework, key library versions)
```

**Step 2: Cross-reference paper vs. code**
- Use the same claim-extraction logic as `/audit`
- Note every underspecified detail — where the paper leaves gaps

**Step 3: Estimate compute**
Based on model size, dataset size, and reported training time (if given), estimate:
- GPU-hours needed
- Memory requirements
- Approximate cost on common cloud providers (A100, RTX 4090, etc.)

---

### PHASE 2 — VERIFIER AGENT

For each underspecified detail:
- Search for the most common default in the field for that parameter
- Cite the source of the suggested default
- Flag the assumption explicitly as a potential divergence point

---

### PHASE 3 — WRITER AGENT (Replication Plan)

```
# Replication Plan: [Paper Title]
**arXiv:** [URL] | **Code:** [URL or "Not found"]
**Target claim:** [The specific result being replicated]

---

## Requirements
| Resource | Specification |
|----------|--------------|
| GPU | [type, VRAM needed, count] |
| Training time | [estimated hours] |
| Estimated cost | [cloud estimate] |
| Key libraries | [framework, versions] |
| Dataset | [name, size, source URL] |

---

## Environment Setup
```bash
# Step-by-step environment setup
pip install torch==X.X.X ...
# or conda commands
```

---

## Step-by-Step Replication Plan

### Step 1: Data Preparation
[Exact instructions for downloading and preprocessing the dataset]
- Source: [URL]
- Preprocessing: [specific steps from paper/code]
- Expected output: [what the preprocessed data should look like]

### Step 2: Model Implementation
[Architecture details, what to implement or verify in the existing code]
- Architecture: [exact spec]
- Deviations from paper to check: [list]

### Step 3: Training
[Exact training configuration]
```python
# Key config parameters
lr = X
batch_size = X
epochs = X
# ...
```

### Step 4: Evaluation
[Exact evaluation protocol]
- Metric: [how it is computed]
- Test set: [which split, how filtered]
- Averaging: [how final numbers are computed]

### Step 5: Comparing Results
[What numbers to compare, what tolerance is acceptable]
- Target: [reported result from paper]
- Acceptable delta: [what counts as successful replication]

---

## Underspecified Details (Assumptions Made)
| Parameter | Paper Says | Assumed Value | Source of Assumption | Risk |
|-----------|-----------|---------------|---------------------|------|
| Dropout rate | Not stated | 0.1 | Common default in similar models ([citation]) | Low |
| ... | ... | ... | ... | ... |

---

## Risk Assessment
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| LR schedule not fully specified | High | High | Try cosine decay with 5% warmup |
| Different hardware = different results | Medium | Medium | Report hardware used; run 3 seeds |
| ... | ... | ... | ... |

---

## Success Criteria
[What results would constitute a successful replication?]
- Primary: Reproduce [metric] within ±X% of reported [value]
- Secondary: Training curve shape matches paper's Figure N
- Failure threshold: If results deviate by more than X%, investigate before proceeding

---

## Suggested Debugging Order if Results Diverge
1. Check LR schedule implementation first
2. Verify dataset preprocessing matches exactly
3. Check batch size vs. gradient accumulation equivalence
4. Run with paper's reported seed if available
5. Compare intermediate representations / loss curves
```

---

## Customization Notes
- User can target a full paper or a specific claim
- If no code exists, produce a from-scratch implementation plan
- Flag when a claim involves non-public data (e.g., proprietary datasets) — replication may be impossible
- Note when the compute requirements are prohibitive for a single GPU
