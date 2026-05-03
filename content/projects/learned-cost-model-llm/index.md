---
title: 'A Generalized Learned Cost Model for LLM Inference'
date: 2025-12-01
summary: |
  Graph-aware GNN cost model that predicts LLM forward-pass latency on
  unseen (model, GPU) combinations from a transformer graph + a 5-dim
  hardware spec vector. Wins all 6 leave-one-model-out splits (28.1% vs.
  XGBoost 70.9% MAPE). Reaches 88.9% top-1 / 100% top-2 GPU-selection
  accuracy — eliminating exhaustive profiling.
tags:
  - ML Systems
  - LLM Inference
  - Cost Model
  - GNN / GAT
  - Performance Prediction
featured: true
links:
  - type: source
    url: https://github.com/XLOverflow/mlsys_course_project
---

Course project for **CMU 11-667 / MLSys**. We tackle a deployer's question: *given a new transformer model and a GPU SKU we have **never profiled together**, how fast will inference run?*

## Problem

Modern learned cost models guide compiler scheduling and runtime placement, but they typically assume **homogeneous hardware** and **per-task retraining** — assumptions that break on the rapidly diversifying GPU fleet (A10, A100, L4, H100, B200) used to serve LLMs. What deployers actually need is latency prediction on a $(\text{model}, \text{GPU})$ **combination that was never profiled** — a new model on an existing GPU, or an existing model on a newly-launched GPU SKU.

## Method

A **graph-aware GNN cost model** that takes:

- $G$ — the transformer computation graph
- $s$ — inference configuration (batch size, seq length, prefill / decode)
- $h$ — a **5-dim continuous hardware spec vector** (FLOPs, memory bandwidth, etc.)

and predicts forward-pass latency $\hat{T}$. The continuous hardware embedding is what enables generalization to unseen GPUs.

We further build a **two-tier SHAP-driven router** that dispatches each query between the GNN and an XGBoost baseline — getting the best of both worlds.

## Results

Trained on **1,360 prefill measurements** across **5 GPUs × 6 transformer models**:

| Split | GNN MAPE | XGBoost MAPE |
| --- | --- | --- |
| Leave-one-model-out (prefill, 6 splits) | **28.1%** | 70.9% |
| Leave-one-model-out (decode, 3 splits) | **8.9%** | 58.2% |
| Leave-one-GPU-out (5 splits) | 34.6% | 40.1% |
| Heterogeneous mixed test | — | 59.1% / 62.0% |
| **Two-tier SHAP router** | **52.3%** (strict win) | — |

For the **GPU-selection task** — picking the fastest GPU for a given $(\text{model}, \text{batch}, \text{seq})$ workload:

- **Top-1 accuracy: 88.9%**
- **Top-2 accuracy: 100%** — the actual fastest GPU is always in the top-2 picks, eliminating exhaustive profiling.

## Stack

PyTorch · PyTorch Geometric (GAT) · XGBoost · SHAP · A10 / A100 / L4 / H100 / B200
