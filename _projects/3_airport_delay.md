---
layout: page
title: Airport Delay Network
description: Found which US airports spread morning delays and which absorb them in the evening, using time-conditioned PageRank. Data hackathon Excellence Award.
importance: 4
img: /assets/img/projects/3_airport_delay.jpg
category: research
period: Nov 2025
context: CAU × Sogang University Data Hackathon
role: Insight discovery and network modeling
outcome:
  - Excellence Award
highlight: Excellence Award
materials:
  - label: Hackathon slides (PDF, Korean)
    url: /assets/slides/airport-delay.pdf
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

The task was to predict whether a US domestic flight arrives 15 or more minutes late, from about 146,000 flights (2018–2022). Instead of treating each flight on its own, I asked how delays move through the airport network: which airports start delays, and which ones collect them.

## Approach

### Delays build up during the day

Departures at 5–6 a.m. are delayed least, and the delay rate climbs through the day to about 25% in the evening. This suggested two different roles: airports that push delays out in the morning, and airports where delays pile up by evening.

<figure>
  <img src="{{ '/assets/img/projects/airport_hourly.jpg' | relative_url }}" alt="Delay rate by departure and arrival hour" style="max-width: 640px;">
  <figcaption>Delay rate by departure hour (top) and arrival hour (bottom).</figcaption>
</figure>

### Busier is not always later

We scored congestion as the number of departures and arrivals at an airport in a given hour. Average arrival delay rises from 4.4 minutes in the least congested fifth to 15.8 minutes in the fourth, but drops slightly in the most congested fifth. The airports in that top group (ATL, DFW, ORD, LAX, and others) delayed less than expected, which points to their operational maturity rather than congestion alone.

<figure>
  <img src="{{ '/assets/img/projects/airport_congestion.jpg' | relative_url }}" alt="Average arrival delay by congestion quantile">
  <figcaption>Average arrival delay by airport-hour congestion quantile.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/airport_q5.jpg' | relative_url }}" alt="Delay residual for the most congested airports">
  <figcaption>In the most congested group, every airport's actual delay was below the expected delay.</figcaption>
</figure>

### Network model

- Built two directed airport graphs: **morning departures (6–10 a.m.)** and **evening arrivals (6–10 p.m.)**.
- Edge weight is the route's delay rate with Bayesian smoothing, so routes with few flights do not get extreme weights.
- Ran weighted PageRank on each graph to get a **morning propagation risk** and an **evening accumulation risk** for every airport.
- The difference between the two scores sorts airports into "pure propagators" and "pure accumulators".

<figure>
  <img src="{{ '/assets/img/projects/airport_network.jpg' | relative_url }}" alt="Morning propagator and evening accumulator networks">
  <figcaption>Left: morning departure network, colored by propagation risk. Right: evening arrival network, colored by accumulation risk.</figcaption>
</figure>

## Results

- Large hubs carry the most risk in both graphs (DEN, DFW, ORD, ATL, CLT).
- **Propagators:** big hubs that scatter flights from the center (DEN, DFW, MIA), and leisure airports such as St. Thomas, Eagle County, and Honolulu. These look quiet, but return trips with families and heavy baggage seem to start delays.
- **Accumulators:** Anchorage, Atlanta, Las Vegas, and several eastern airports, where many afternoon flights arrive as final destinations because of time zones.
- The same hub can play a different role at different times of day, which a single per-airport delay rate hides.

<figure>
  <img src="{{ '/assets/img/projects/airport_roles.jpg' | relative_url }}" alt="Propagator versus accumulator scatter plot" style="max-width: 560px;">
  <figcaption>Each dot is an airport (382 in total). Red: top 10 propagators. Blue: top 10 accumulators.</figcaption>
</figure>

The PageRank scores were then added as features to a gradient-boosted classifier, alongside time, airline, and aircraft features.

### Recommendations

- **Propagator airports:** put check-in and security staff on the morning peak, and use incentives for early and late flights to spread traffic.
- **Accumulator airports:** add baggage and ground crews and public transport in the afternoon, and turn simple arrivals into transfer connections.

## Photos

<figure>
  <img src="{{ '/assets/img/projects/airport_award.jpg' | relative_url }}" alt="Excellence Award plaque, CAU × Sogang University Data Hackathon" style="max-width: 420px;">
  <figcaption>Excellence Award, CAU × Sogang University Data Hackathon (Nov 15, 2025).</figcaption>
</figure>
