---
layout: post
title: "Seminar: Diffusion Models, from DDPM to Diffusion LLMs"
date: 2026-08-05 00:00:00
description: "Lab seminar slides on diffusion basics (DDPM, LDM, DiT, flow matching, MMDiT) and two recent uses: video-to-audio (MMAudio) and diffusion language models (LLaDA)"
tags: [seminar, diffusion, generative-models]
categories: [Research]
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

## Overview

A lab seminar at the NLP & ISDS Lab on how diffusion models work and where they are used now. The talk is in Korean.

| Part | Topics |
| --- | --- |
| Generative models | Modeling the data distribution, and why the normalizing constant forces workarounds |
| Diffusion basics | Forward and reverse process, DDPM training and sampling, LDM, DiT |
| Backbones | Flow matching (predicting velocity instead of noise), MMDiT |
| Use 1: Video-to-Audio | MMAudio |
| Use 2: Diffusion LLMs | Why diffusion does not transfer directly to text, masked diffusion (LLaDA), and the debate on decoding order |

---

## Slides

[Download the slides (PDF)]({{ '/assets/seminar/diffusion/slides.pdf' | relative_url }})

<iframe src="{{ '/assets/seminar/diffusion/slides.pdf' | relative_url }}" width="100%" height="560" style="border: 1px solid #ccc;"></iframe>

## Takeaways

- Diffusion learns to undo noise step by step, which avoids computing the normalizing constant of the data distribution.
- Working in a latent space (LDM) and using a Transformer backbone (DiT, MMDiT) made diffusion scale. Flow matching straightens the path from noise to data.
- In language, diffusion LLMs can fill tokens in any order. Recent work (Ni et al., ICML 2026) questions whether that freedom helps reasoning: samplers tend to fix easy, high-confidence tokens first and postpone the tokens that decide the direction of the answer, and any-order generation also makes RL training harder. Training with RL in left-to-right order and decoding in parallel afterward is one proposed answer.

## References

1. J. Ho, A. Jain, P. Abbeel. _Denoising Diffusion Probabilistic Models._ NeurIPS 2020.
2. R. Rombach et al. _High-Resolution Image Synthesis with Latent Diffusion Models._ CVPR 2022.
3. W. Peebles, S. Xie. _Scalable Diffusion Models with Transformers._ ICCV 2023.
4. Y. Lipman et al. _Flow Matching for Generative Modeling._ ICLR 2023.
5. P. Esser et al. _Scaling Rectified Flow Transformers for High-Resolution Image Synthesis._ ICML 2024.
6. H. K. Cheng et al. _MMAudio: Taming Multimodal Joint Training for High-Quality Video-to-Audio Synthesis._ CVPR 2025.
7. S. Nie et al. _Large Language Diffusion Models._ 2025.
8. Z. Ni et al. _The Flexibility Trap: Rethinking the Value of Arbitrary Order in Diffusion Language Models._ ICML 2026.

Figures in the slides are reproduced from the original papers and from KIAS HORIZON for teaching purposes, and credit belongs to the original authors.
