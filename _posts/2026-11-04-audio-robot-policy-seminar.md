---
layout: post
title: "Paper Seminar: Audio-Aware Robot Policies (MultiGen & HEAR)"
date: 2026-11-04 00:00:00
description: "Seminar slides on two audio-aware robot policy papers: MultiGen (generative audio for sim-to-real) and HEAR (streaming auditory memory)"
tags: [paper-review, seminar, robotics, audio]
categories: [Research]
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

## Overview

Seminar on how robot policies can use sound, built around two papers.

| Bottleneck | Paper | Links |
| --- | --- | --- |
| Sound is hard to simulate (data) | **MultiGen**: The Sound of Simulation, CoRL 2025 | [arXiv](https://arxiv.org/abs/2507.02864) · [Project](https://multigen-audio.github.io) · [Code](https://github.com/renwang435/multigen) |
| Short sounds are missed while acting (architecture) | **HEAR**: Towards the Vision-Sound-Language-Action Paradigm, IJRR 2026 | [arXiv](https://arxiv.org/abs/2603.16086) · [Project](https://hear.irmv.top) · [Code](https://github.com/IRMVLab/HEAR) · [Dataset](https://huggingface.co/datasets/biubiu2/OpenX-Sound) |

---

## Slides

[Download the slides (PDF)]({{ '/assets/seminar/audio-robot-policy/slides.pdf' | relative_url }})

<iframe src="{{ '/assets/seminar/audio-robot-policy/slides.pdf' | relative_url }}" width="100%" height="560" style="border: 1px solid #ccc;"></iframe>

The videos shown in the talk are not hosted here. Please see the original project pages above.

## References

1. R. Wang et al. *The Sound of Simulation: Learning Multimodal Sim-to-Real Robot Policies with Generative Audio.* CoRL 2025. arXiv:2507.02864.
2. C. Nie, T. Deng, G. Wang, Z. Liu, H. Wang. *Towards the Vision-Sound-Language-Action Paradigm: The HEAR Framework for Sound-Centric Manipulation.* IJRR 2026. arXiv:2603.16086.

Figures from MultiGen (Wang et al., CoRL 2025) are used under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Figures from HEAR (Nie et al., IJRR 2026) are reproduced for review purposes, and credit belongs to the original authors.
