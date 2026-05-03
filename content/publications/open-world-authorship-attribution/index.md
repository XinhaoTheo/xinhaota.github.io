---
title: 'Open-World Authorship Attribution'

authors:
  - me
  - Songhua Liu
  - Cong Xia
  - Kunjun Li
  - Xinchao Wang

author_notes:
  - ''
  - ''
  - ''
  - ''
  - 'Corresponding author'

date: '2025-07-27T00:00:00Z'
publishDate: '2025-07-27T00:00:00Z'

publication_types: ['paper-conference']

publication: 'In *Findings of the Association for Computational Linguistics: ACL 2025*'
publication_short: 'In *ACL 2025 Findings*'

abstract: |
  Recent years have witnessed rapid advancements in Large Language Models
  (LLMs). Nevertheless, it remains unclear whether state-of-the-art LLMs
  can infer the author of an anonymous research paper solely from the
  text, without any additional information. To investigate this novel
  challenge, which we define as Open-World Authorship Attribution, we
  introduce a benchmark comprising thousands of research papers across
  various fields to quantitatively assess model capabilities. Then, at
  the core of this paper, we tailor a two-stage framework to tackle this
  problem: candidate selection and authorship decision. Specifically, in
  the first stage, LLMs are prompted to generate multi-level key
  information, which are then used to identify potential candidates
  through Internet searches. In the second stage, we introduce key
  perspectives to guide LLMs in determining the most likely author from
  these candidates. Extensive experiments on our benchmark demonstrate
  the effectiveness of the proposed approach, achieving 60.7% and 44.3%
  accuracy in the two stages, respectively. We will release our
  benchmark and source codes to facilitate future research in this field.

summary: |
  A two-stage LLM framework that infers the author of an anonymous
  research paper from text alone — multi-level keyword generation for
  candidate retrieval, followed by perspective-guided authorship
  decision. Achieves 60.7% / 44.3% accuracy on the proposed benchmark.

tags:
  - Large Language Models
  - Authorship Attribution
  - Information Retrieval
  - Benchmarks

featured: true

hugoblox:
  ids:
    arxiv: ''

links:
  - type: pdf
    url: 'https://aclanthology.org/2025.findings-acl.913.pdf'
  - type: custom
    name: ACL Anthology
    url: 'https://aclanthology.org/2025.findings-acl.913/'

image:
  caption: 'Two-stage framework: keyword generation & candidate collection, followed by author decision.'
  focal_point: ''
  preview_only: false

projects: []
slides: ''
---
