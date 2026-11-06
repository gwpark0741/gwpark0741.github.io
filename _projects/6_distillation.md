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

For the class we built a live demo on MNIST with the original dataset distillation code and the distilled images released by its authors (Wang et al., 2018). The ten images below, one per digit, are learned so that a LeNet trained on them generalizes to real digits.

<figure>
  <img src="{{ '/assets/img/projects/distill_mnist.jpg' | relative_url }}" alt="Ten distilled MNIST images, one per class" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Distilled images, one per class. They look nothing like digits to us.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/distill_compare.jpg' | relative_url }}" alt="Original MNIST versus ten distilled images" style="width: 100%;">
</figure>

In our run, starting from a random network (8.25% test accuracy), three gradient steps on these ten images raised accuracy to **88.66%** on the 10,000 test images, in under a second. That is about 90% of what training on all 60,000 images reaches, with 6,000 times less data.

<figure>
  <img src="{{ '/assets/img/projects/distill_predictions.jpg' | relative_url }}" alt="Predictions on test digits" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Predictions on real test digits after training on the ten images.</figcaption>
</figure>

## Trajectory matching

The main method we studied is Matching Training Trajectories (MTT). A teacher network is trained on the real data, and a student network is trained on the synthetic data. The synthetic pixels are updated so that the student's parameter trajectory follows the teacher's, which packs the dataset's key features into a few images.

<figure>
  <img src="{{ '/assets/img/projects/distill_s6.jpg' | relative_url }}" alt="MTT paper and its distilled images" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Distilled images from MTT on CIFAR-100, Tiny ImageNet, and ImageNet subsets.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/distill_s7.jpg' | relative_url }}" alt="Teacher and student trajectories" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Two trajectories: the teacher trained on real data and the student trained on distilled data.</figcaption>
</figure>

This is a bi-level optimization. The inner loop trains the student on the synthetic images; the outer loop updates the synthetic images so the trained student does well on real data.

<figure>
  <img src="{{ '/assets/img/projects/distill_s8.jpg' | relative_url }}" alt="Inner and outer loop of trajectory matching" style="width: 100%;">
</figure>

## Where trajectory matching breaks down

<figure>
  <img src="{{ '/assets/img/projects/distill_s9.jpg' | relative_url }}" alt="Limits of trajectory matching" style="width: 100%;">
</figure>

- **Architecture dependence:** the images imitate one model's trajectory, so they are biased toward that model.
- **Compute cost:** it needs expert trajectories first, then unrolls many training steps and backpropagates through them.
- **Complex data:** on larger, more varied datasets the trajectories become unstable and the pixel values are hard to fit.

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
