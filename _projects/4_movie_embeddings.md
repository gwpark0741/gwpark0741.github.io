---
layout: page
title: Movie Recommendation with Item Embeddings
description: Learned movie embeddings from co-watched pairs with genre information, then recommended unseen movies from a user's taste vector.
importance: 6
img: # 왼쪽 사진. 예: /assets/img/projects/4_movie_embeddings.jpg
category: coursework
period: "2025"
context: Basic Recommender Systems (course)
outcome:
  - A+
highlight: A+
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

The goal was to learn an embedding for every movie with a neural network, then use those embeddings to recommend movies to each user.

## Approach

### Learning movie embeddings

On MovieLens, I kept the 450 movies with at least 50 ratings and turned each user's history into pairs of movies they both watched (3.47 million training pairs). The network takes one movie and predicts the other movie in the pair, so movies watched by the same people end up with similar embeddings, in the spirit of item2vec.

Beyond the required input–embedding–output network, I added each movie's 19 genres as a multi-hot vector next to its 64-dimensional embedding, followed by two hidden layers (256 and 128 units) with batch normalization and dropout.

### Recommending to a user

A user's taste vector is the average of the embeddings of the movies they watched. The model ranks every unwatched movie by cosine similarity to that vector and returns the top ten with title, genre, and score.

## Results

Predicting the exact co-watched movie out of 450 is hard: top-5 accuracy on held-out pairs was 2.6%, about 2.4 times random.

## Limits and next steps

The embeddings are useful for ranking, but the task rewards popular movies, and averaging a whole history blurs a user's distinct tastes. A ranking loss with negative sampling and evaluation on held-out user interactions would fit recommendation better, which I tried in a later version.
