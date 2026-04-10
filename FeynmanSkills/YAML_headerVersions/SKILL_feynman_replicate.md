---
name: feynman-replicate
description: Feynman replication planning workflow. Activate when user types /replicate, or asks to "plan a replication", "reproduce this result", "how would I implement this paper", "what do I need to run this experiment", or "write a replication plan for arxiv:XXXXXXX". Produces a detailed step-by-step replication plan with hardware requirements, underspecified details flagged, risk assessment, and success criteria.
---

# WORKFLOW: /replicate

## What This Workflow Produces
A detailed, actionable replication plan: step-by-step from environment setup through final evaluation, with hardware requirements, identified underspecifications, risk assessment, and success criteria. Claude produces the plan — the user executes it.

---

## Execution Protocol

### PHASE 1 — 🔍 RESEARCHER AGENT (announce this phase)

**Step 1: Get everything**
- Fetch paper: `https://arxiv.org/abs/<ID>`
- Find and fetch codebase (same strategy as /audit)
- Extract every detail needed for reproduction:

```
REPRODUCTION CHECKLIST:
□ Model architecture (layers, dims, activations, normalization)
□ Optimizer (name, lr, schedule, warmup, weight decay, gradient clipping)
□ Training duration (steps/epochs, batch size, gradient accumulation)
□ Dataset (name, version, splits, preprocessing, augmentation)
□ Evaluation protocol (metric, test set, averaging strategy)
□ Hardware (GPU type, count, training time reported)
□ Random seeds
□ Mixed precision settings (bf16/fp16)
□ Key hyperparameters (temperature, dropout, etc.)
□ Environment (framework, key library versions)
```

**Step 2: Cross-reference paper vs. code**
Note every underspecified detail — where the paper leaves gaps.

**Step 3: Estimate compute**
Based on model size, dataset size, reported training time:
- GPU-hours needed
- Memory requirements
- Approximate cost on common cloud providers (A100, H100, RTX 4090)

---

### PHASE 2 — ✅ VERIFIER AGENT (announce this phase)

For each underspecified detail:
- Search for the most common default in the field for that parameter
- Cite the source of the suggested default
- Flag the assumption explicitly as a potential divergence point

---

### PHASE 3 — ✍️ WRITER AGENT (announce this phase)

```
# Replication Plan: [Paper Title]
**arXiv:** [URL] | **Code:** [URL or "Not found"]
**Target claim:** [The specific result being replicated]

## Requirements
| Resource | Specification |
|----------|--------------|
| GPU | [type, VRAM, count] |
| Training time | [estimated hours] |
| Estimated cost | [cloud estimate] |
| Key libraries | [framework + versions] |
| Dataset | [name, size, source URL] |

## Environment Setup
```bash
pip install torch==X.X.X ...
```

## Step-by-Step Replication Plan

### Step 1: Data Preparation
[Exact instructions for downloading and preprocessing]
- Source: [URL]
- Preprocessing: [specific steps from paper/code]
- Expected output: [what the data should look like]

### Step 2: Model Implementation
[Architecture details, what to verify in existing code]

### Step 3: Training
```python
lr = X
batch_size = X
epochs = X
```

### Step 4: Evaluation
[Exact evaluation protocol, metric computation, test set]

### Step 5: Comparing Results
- Target: [reported result]
- Acceptable delta: [what counts as successful replication]

## Underspecified Details (Assumptions Made)
| Parameter | Paper Says | Assumed Value | Source | Risk |
|-----------|-----------|---------------|--------|------|
| Dropout | Not stated | 0.1 | Common default ([citation]) | Low |

## Risk Assessment
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| LR schedule not fully specified | High | High | Try cosine decay + 5% warmup |

## Success Criteria
- Primary: Reproduce [metric] within ±X% of reported [value]
- Secondary: Training curve shape matches paper Figure N
- Failure threshold: If >X% deviation, investigate before proceeding

## Suggested Debugging Order if Results Diverge
1. Check LR schedule implementation
2. Verify dataset preprocessing
3. Check batch size vs. gradient accumulation equivalence
4. Run with paper's reported seed if available
5. Compare loss curves against paper figures
```

---

## Customization
- User can target a full paper or a specific claim
- If no code exists: produce a from-scratch implementation plan
- Flag when a claim involves non-public data — replication may be impossible
- Note when compute requirements are prohibitive for a single GPU
