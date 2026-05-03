---
title: Event Extraction with Large Language Models
date: 2025-08-01
summary: |
  End-to-end event-type classification pipeline for short financial messages
  — RAG-based dynamic few-shot prompting plus LoRA fine-tuning of Qwen-3 4B
  scaled with DeepSpeed ZeRO-2/3. Achieved ~0.75 Macro-F1, +300% absolute
  points over the base model.
tags:
  - LLM
  - LoRA
  - DeepSpeed
  - RAG
  - LangChain
  - Fine-tuning
featured: true
links:
  - type: source
    url: https://github.com/XinhaoTheo/Event-Extraction-with-Large-Language-Models
---

End-to-end pipeline for **event-type classification of short financial messages** (label mapping → inference → evaluation), built between May and August 2025.

## Highlights

- **Robust prompt-only baseline.** Handled ambiguous labels with **self-consistency** and **multi-vote decoding** over multiple prompt variants.
- **RAG-based dynamic few-shot prompting.** Built a LangChain retrieval layer that pulls similar labeled exemplars per input, achieving **0.60+ Macro-F1** and substantially better tail-class stability than the prompt-only baseline.
- **Supervised fine-tuning.** Fine-tuned **Qwen-3 4B** with **LoRA** and scaled training across multiple GPUs using **DeepSpeed (ZeRO-2 / ZeRO-3)**, reaching **~0.75 Macro-F1** — a **+300% absolute** improvement over the base model.

## Stack

PyTorch · HuggingFace · PEFT/LoRA · DeepSpeed · LangChain · Qwen-3
