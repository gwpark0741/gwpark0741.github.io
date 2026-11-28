---
layout: page
title: Squat Form Assessment
description: Scores each squat rep from a phone video on five form criteria. Compared XGBoost, LSTM, and a video ViT.
importance: 5
img: /assets/img/projects/5_posture.jpg
category: research
period: "2025"
context: Parrot (Sogang University Data Science Society)
role: Keypoint preprocessing for the LSTM, the MediaPipe test environment, occlusion handling, and the web demo
materials:
  - label: Project slides (PDF, Korean)
    url: /assets/slides/squat-form.pdf
---

<link rel="stylesheet" href="{{ '/assets/project-page.css' | relative_url }}">

<table class="project-info">
  <tr><th>Period</th><td>{{ page.period }}</td></tr>
  <tr><th>Context</th><td>{{ page.context }}</td></tr>
  {% if page.role %}<tr><th>Role</th><td>{{ page.role }}</td></tr>{% endif %}
  {% if page.outcome %}<tr><th>Outcome</th><td>{{ page.outcome | join: "<br>" }}</td></tr>{% endif %}
  {% if page.materials %}<tr><th>Materials</th><td>{% for m in page.materials %}<a href="{{ m.url | relative_url }}">{{ m.label }}</a>{% unless forloop.last %}<br>{% endunless %}{% endfor %}</td></tr>{% endif %}
</table>

## Problem

An AI home-training coach: the user films a squat from the front or the side, and the system scores each rep with feedback.

- **Side view:** neutral spine, heels on the ground, range of motion
- **Front view:** head and gaze alignment, knee alignment

## Approach

### Data and preprocessing

We used the AI Hub fitness posture dataset, which has body keypoint coordinates from five cameras with true/false labels for each form condition.

<figure>
  <img src="{{ '/assets/img/projects/squat_s4.jpg' | relative_url }}" alt="Dataset overview">
</figure>

Each video was split into reps (top, lowest point, top), keypoints were turned into joint angles, and every rep was interpolated and smoothed to the same 64 frames so the models get a fixed-length input.

<figure>
  <img src="{{ '/assets/img/projects/squat_s9.jpg' | relative_url }}" alt="Rep separation">
</figure>

<figure>
  <img src="{{ '/assets/img/projects/squat_s12.jpg' | relative_url }}" alt="Joint angles used for each view">
  <figcaption>Joint angles computed for the side and front views.</figcaption>
</figure>

### Models

We compared an XGBoost baseline on per-frame features, an LSTM on angle sequences, and a video ViT (VideoMAE) on raw frames.

<figure>
  <img src="{{ '/assets/img/projects/squat_s21.jpg' | relative_url }}" alt="LSTM versus ViT pipelines">
  <figcaption>Keypoints and engineered angles into an LSTM (left) versus raw frames into a ViT (right).</figcaption>
</figure>

### Making it work on real user videos

The training videos were clean, but phone videos are not. In side views, the far leg is often hidden behind the near one, so MediaPipe loses the far knee and the angle cannot be computed. I built the test environment that runs the whole pipeline on a user's video, and handled this case by splitting reps from the visible knee angle and applying the same rep index to the other view.

<figure>
  <img src="{{ '/assets/img/projects/squat_s20.jpg' | relative_url }}" alt="Test pipeline from user video to feedback">
  <figcaption>Test pipeline: upload, MediaPipe keypoints, angles, rep split, resampling to 64 frames, model inference, feedback.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/squat_s19.jpg' | relative_url }}" alt="Occlusion handling">
</figure>

## Results

On test videos, the LSTM judged neutral spine and head alignment more steadily than XGBoost, because it looks at the whole movement rather than single frames. It was stricter on heel contact, since small shaking that lasts over time looks like "not grounded" to a sequence model. Simple angle criteria such as range of motion and knee alignment were near 99 points for both.

<figure>
  <img src="{{ '/assets/img/projects/squat_s25.jpg' | relative_url }}" alt="XGBoost versus LSTM scores per criterion">
</figure>

<figure>
  <img src="{{ '/assets/img/projects/squat_s26.jpg' | relative_url }}" alt="Why the models differ">
  <figcaption>Why the models differ, and the fix: remove noise in preprocessing and tune the tolerance threshold.</figcaption>
</figure>

## Limits and next steps

Next steps: choose the model per criterion, try an ensemble, and retrain the deep models as more data becomes available.
