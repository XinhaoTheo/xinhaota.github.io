---
title: 'Step-Level Weighted Distillation for Chain-of-Thought Speculative Decoding'
date: 2025-12-01
summary: |
  Speculative decoding for CoT reasoning. We upweight reasoning steps during
  EAGLE draft-head training using per-step acceptance rate (alignment
  difficulty) and process-reward-model scores (reasoning importance),
  achieving up to 4.61× wall-clock speedup on V100 — over the uniform-CoT
  EAGLE baseline (4.27×) and a compute-matched control (4.52×), with zero
  accuracy loss.
tags:
  - Speculative Decoding
  - LLM Inference
  - Knowledge Distillation
  - EAGLE
  - Chain-of-Thought
  - ML Systems
featured: true
links:
  - type: source
    url: https://github.com/XLOverflow/anlp_course_project
---

Course project for **CMU 11-711 Advanced NLP**. We bridge two independent lines of work — **step-level reasoning distillation** and **speculative-decoding draft-model distillation** — by introducing a **step-weighted KL loss** for EAGLE draft-head training.

## Problem

Chain-of-Thought reasoning makes LLMs much smarter, but the long token sequences are expensive to decode. Speculative decoding (drafting-and-verifying) mitigates the latency, but existing EAGLE-style draft models are trained **uniformly over tokens** — they ignore the *step structure* of reasoning.

## Method

During EAGLE draft-head training we upweight each token's KL loss by a per-step weight:

$$w_s = 1 + \alpha\,(1 - \beta_s) + \gamma \cdot \mathrm{PRM}'_s$$

derived from two complementary signals:

- **Alignment difficulty** — per-step SD acceptance rate $\beta_s$, profiled offline from a uniform draft. Steps the draft frequently misses get extra weight.
- **Reasoning importance** — Qwen2.5-Math-PRM-7B step scores. Pivotal reasoning steps get extra weight.

Target model is **Qwen3-8B** (frozen); draft head is **EAGLE-3 (0.5B)**. Training data: GSM8K + MATH500 CoT traces.

## Results

| Method | Speedup (V100) |
| --- | --- |
| EAGLE-orig (general-domain) | baseline |
| Uniform CoT fine-tuned EAGLE | 4.27× |
| Compute-matched "continue uniform" | 4.52× |
| **Step-weighted distillation (ours)** | **4.61×** |

- **Zero accuracy degradation** vs. the target model.
- The gap between step-weighted and uniform training **widens on MATH500** (harder, more diverse reasoning), suggesting step weighting matters most when the reasoning distribution is diverse.
- Surprisingly, **combining both signals gives no gain** over the better single signal — pivotal reasoning steps (high PRM) coincide with the steps drafts miss (low $\beta_s$); the two signals are empirically redundant.

## Stack

PyTorch · EAGLE-3 · Qwen3-8B · Qwen2.5-Math-PRM-7B · CUDA · V100
