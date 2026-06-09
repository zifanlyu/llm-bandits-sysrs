# LLM-Bandits: Cutting LLM Evaluation Costs with SySRs

This repository contains the codebase for the paper:

> **Cutting LLM Evaluation Costs with SySRs: A Bandit Algorithm that Provably Exploits Model Similarity**  
> Zifan Lyu, Chahine Nejma, Tobias Wegel, Fanny Yang, Florian Dorner 
> ICML 2026 — [https://icml.cc/virtual/2026/poster/60876](https://icml.cc/virtual/2026/poster/60876)  
> arXiv — [https://arxiv.org/abs/2606.07726](https://arxiv.org/abs/2606.07726)

## Overview

Large Language Models are commonly benchmarked by evaluating all candidate models on every query in a test set. This can be wasteful when a practitioner only wants to find the best model to deploy. This work proposes **Synchronized Successive Rejects (SySRs)**, a best-arm identification (BAI) bandit algorithm that exploits the similarity between LLM responses to the same prompt. SySRs is hyperparameter-free, comes with provable performance guarantees, and empirically outperforms all baselines on 15 standard benchmarks.

---

## Repository Structure

```
.
├── bai_algs.py                        # Core BAI algorithm implementations
├── ucb_lrf_vectorized.py              # UCB-E-LRF (low-rank factorization) implementation
├── datasets/                          # Dataset loading utilities and constants
│   └── MODEL_CANDIDATES_AND_EVALUATION_PARAMETERS_BY_DATASET.md
├── main_results/                      # Experiment 1: Best Arm Identification
│   ├── run_bai_experiments.py
│   ├── run_lrf_experiments.py
│   ├── plot_bai_combined_figure.py
│   └── compute_bai_confidence_budget.py
├── subset_selection_comparison/       # Experiment 2: Subset Selection
│   ├── run_subset_selection_experiments.py
│   └── plot_subset_selection.py
└── top_k_analysis/                    # Experiment 3: Top-k Identification & Ranking
    ├── run_top_k_identification_experiments.py
    └── plot_top_k_identification.py
```

---

## Experiments

There are three main sets of experiments, each in its own subdirectory.

### 1. Main Results — Best Arm Identification (`main_results/`)

Compares SySRs (Smart-SR / Smart-UCB-E) against baselines (Naive-Baseline, Uniform-Pulls, SR, UCB-E) on the task of identifying the single best LLM across 15 datasets. Also includes the UCB-E-LRF (low-rank factorization) variant.

**Step 1 — Run experiments:**
```bash
# All 15 datasets
python main_results/run_bai_experiments.py --all

# UCB-E-LRF variant (all datasets)
python main_results/run_lrf_experiments.py --all
```

**Step 2 — Plot results:**
```bash
python main_results/plot_bai_combined_figure.py

# Concise mode (5 main algorithms, combined variants — matches paper figure)
python main_results/plot_bai_combined_figure.py --concise
```

**Step 3 — Compute Confidence Budget (CB) statistics:**
```bash
python main_results/compute_bai_confidence_budget.py
```

---

### 2. Subset Selection Comparison (`subset_selection_comparison/`)

Compares SySRs against subset-selection baselines on the BAI task.

**Step 1 — Run experiments:**
```bash
python subset_selection_comparison/run_subset_selection_experiments.py --all
```

**Step 2 — Plot results:**
```bash
python subset_selection_comparison/plot_subset_selection.py
```

---

### 3. Top-k Identification & Ranking (`top_k_analysis/`)

Evaluates algorithms on the harder task of identifying the top-*k* set of LLMs, and on correctly ranking the top-*k* models, across all 15 datasets.

**Step 1 — Run experiments:**
```bash
python top_k_analysis/run_top_k_identification_experiments.py --all
```

**Step 2 — Plot results:**
```bash
# Top-k identification rate
python top_k_analysis/plot_top_k_identification.py --metric 

# Top-k ranking rate
python top_k_analysis/plot_top_k_identification.py --metric top_m_ranking_rate_elim

# Per-rank accuracy
python top_k_analysis/compute_top_k_confidence_budget.py --metric per_rank_accuracy_elim
```


---

## Datasets

The experiments use **15 benchmark datasets**. The datasets span a range of tasks including commonsense reasoning, mathematics, medical QA, legal reasoning, language translation, and more. A full breakdown of candidate model counts per dataset is provided in [`datasets/MODEL_CANDIDATES_AND_EVALUATION_PARAMETERS_BY_DATASET.md`](datasets/MODEL_CANDIDATES_AND_EVALUATION_PARAMETERS_BY_DATASET.md).

| Dataset | Source | # Candidate Models |
|---|---|---:|
| commonsense | HELM Lite | 80 |
| gsm | HELM Lite | 80 |
| legalbench | HELM Lite | 80 |
| math | HELM Lite | 80 |
| med_qa | HELM Lite | 80 |
| mmlu | HELM Lite | 80 |
| narrative_qa | HELM Lite | 80 |
| natural_qa | HELM Lite | 80 |
| wmt_14 | HELM Lite | 80 |
| arc_challenge | Open LLM Leaderboard | 218 |
| bbh | Open LLM Leaderboard | 39 |
| gpqa | Open LLM Leaderboard | 450 |
| ifeval | Open LLM Leaderboard | 450 |
| mmlu_pro | Open LLM Leaderboard | 48 |
| musr | Open LLM Leaderboard | 450 |

**Attribution:** We did not run any model inference ourselves. The per-model, per-query evaluation scores were sourced from publicly available leaderboards:

- **HELM Lite** (`commonsense`, `gsm`, `legalbench`, `math`, `med_qa`, `mmlu`, `narrative_qa`, `natural_qa`, `wmt_14`): scores from the [HELM Lite leaderboard](https://crfm.stanford.edu/helm/lite/latest/).
- **Open LLM Leaderboard** (`arc_challenge`, `bbh`, `gpqa`, `ifeval`, `mmlu_pro`, `musr`): scores from the [Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard).

---

## Installation

```bash
pip install -r requirements.txt
```

---

## Citation

If you use this codebase, please cite:

```bibtex
@inproceedings{dorner2026sysr,
  title     = {Cutting {LLM} Evaluation Costs with {SySRs}: A Bandit Algorithm that Provably Exploits Model Similarity},
  author    = {Lyu, Zifan and Nejma, Chahine and Wegel, Tobias and Yang, Fanny and Dorner, Florian},
  booktitle = {Proceedings of the 43rd International Conference on Machine Learning (ICML)},
  year      = {2026},
  url       = {https://icml.cc/virtual/2026/poster/60876}
}
```
