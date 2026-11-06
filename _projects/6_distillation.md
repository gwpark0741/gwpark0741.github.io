---
layout: page
title: Diffusion-Based Dataset Distillation
description: Ten synthetic images train MNIST to 88.7% accuracy. Compared trajectory matching with diffusion-based distillation.
importance: 6
meta: "Fall 2025 · AI Data Analysis"
img: /assets/img/projects/6_distillation.jpg
category: coursework
---

<link rel="stylesheet" href="{{ '/assets/project-page.css' | relative_url }}">

**Fall 2025 · AI Data Analysis, joint undergraduate–graduate course (A+) · Team project**

Full materials (Korean): [Course presentation (PDF)]({{ '/assets/slides/dataset-distillation.pdf' | relative_url }})

Dataset distillation compresses a whole training set into a few synthetic images, so a model trained only on them still performs well. We studied how it works, where the classic method breaks down, and how diffusion models help.

## Ten images instead of sixty thousand

We prepared a live demo for the class using the original dataset distillation method on MNIST. The ten images below, one per digit, were learned so that a LeNet trained on them generalizes to real digits.

<figure>
  <img src="{{ '/assets/img/projects/distill_mnist.jpg' | relative_url }}" alt="Ten distilled MNIST images, one per class" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Distilled images, one per class. They look nothing like digits to us.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/distill_compare.jpg' | relative_url }}" alt="Original MNIST versus ten distilled images" style="width: 100%;">
</figure>

Starting from a random network (8.25% test accuracy), three gradient steps on these ten images raised accuracy to **88.66%** on the 10,000 test images, in under a second. That is about 90% of what training on all 60,000 images reaches, with 6,000 times less data.

<figure>
  <img src="{{ '/assets/img/projects/distill_predictions.jpg' | relative_url }}" alt="Predictions on test digits" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Predictions on real test digits after training on the ten images.</figcaption>
</figure>

## Trajectory matching and its limits

Trajectory matching (MTT) makes a student model trained on the synthetic data follow the parameter trajectory of a teacher trained on the real data, and updates the synthetic pixels to close the gap.

<figure>
  <img src="{{ '/assets/img/projects/distill_s6.jpg' | relative_url }}" alt="Trajectory matching" style="width: 100%;">
</figure>

It has three limits: the distilled data depends on the model architecture used to make it, unrolling the training trajectories is expensive, and it does not scale to large, complex datasets.

## Diffusion-based distillation

Diffusion-based methods such as D4M use a pretrained latent diffusion model instead. They encode real images into latents, find a few prototypes per class with k-means, and decode the prototypes back into images with the diffusion model. The result does not depend on one architecture and stays realistic.

<figure>
  <img src="{{ '/assets/img/projects/distill_s10.jpg' | relative_url }}" alt="Diffusion-based distillation pipeline" style="width: 100%;">
</figure>

<figure>
  <img src="{{ '/assets/img/projects/distill_s11.jpg' | relative_url }}" alt="Distilled images from diffusion-based and trajectory matching methods" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Diffusion-based distilled images stay recognizable; trajectory matching images do not.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/distill_s12.jpg' | relative_url }}" alt="Comparison of the two approaches" style="width: 100%;">
</figure>

## References

1. T. Wang, J.-Y. Zhu, A. Torralba, A. A. Efros. _Dataset Distillation._ arXiv:1811.10959, 2018.
2. G. Cazenavette et al. _Dataset Distillation by Matching Training Trajectories._ CVPR 2022.
3. D. Su et al. _D4M: Dataset Distillation via Disentangled Diffusion Model._ CVPR 2024.
