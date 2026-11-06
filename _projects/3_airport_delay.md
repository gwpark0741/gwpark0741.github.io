---
layout: page
title: Airport Delay Network
description: Found which US airports spread morning delays and which absorb them in the evening, using time-conditioned PageRank. Data hackathon Excellence Award.
importance: 3
meta: "Nov 2025 · CAU × Sogang University Data Hackathon"
img: /assets/img/projects/3_airport_delay.jpg
category: research
---

<link rel="stylesheet" href="{{ '/assets/project-page.css' | relative_url }}">

**Nov 2025 · CAU × Sogang University Data Hackathon · Excellence Award · My role: insight discovery and network modeling**

Full materials (Korean): [Hackathon slides (PDF)]({{ '/assets/slides/airport-delay.pdf' | relative_url }})

<figure>
  <img src="{{ '/assets/img/projects/airport_award.jpg' | relative_url }}" alt="Excellence Award plaque, CAU × Sogang University Data Hackathon" style="width: 100%; max-width: 420px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Excellence Award, CAU × Sogang University Data Hackathon (Nov 15, 2025).</figcaption>
</figure>

## Question

The task was to predict whether a US domestic flight arrives 15 or more minutes late, from about 146,000 flights (2018–2022). Instead of treating each flight on its own, I asked how delays move through the airport network: which airports start delays, and which ones collect them.

## Delays build up during the day

Departures at 5–6 a.m. are delayed least, and the delay rate climbs through the day to about 25% in the evening. This suggested two different roles: airports that push delays out in the morning, and airports where delays pile up by evening.

<figure>
  <img src="{{ '/assets/img/projects/airport_hourly.jpg' | relative_url }}" alt="Delay rate by departure and arrival hour" style="width: 100%; max-width: 640px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Delay rate by departure hour (top) and arrival hour (bottom).</figcaption>
</figure>

## Busier is not always later

We scored congestion as the number of departures and arrivals at an airport in a given hour. Average arrival delay rises from 4.4 minutes in the least congested fifth to 15.8 minutes in the fourth, but drops slightly in the most congested fifth. The airports in that top group (ATL, DFW, ORD, LAX, and others) delayed less than expected, which points to their operational maturity rather than congestion alone.

<figure>
  <img src="{{ '/assets/img/projects/airport_congestion.jpg' | relative_url }}" alt="Average arrival delay by congestion quantile" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Average arrival delay by airport-hour congestion quantile.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/airport_q5.jpg' | relative_url }}" alt="Delay residual for the most congested airports" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">In the most congested group, every airport's actual delay was below the expected delay.</figcaption>
</figure>

## Method

- Built two directed airport graphs: **morning departures (6–10 a.m.)** and **evening arrivals (6–10 p.m.)**.
- Edge weight is the route's delay rate with Bayesian smoothing, so routes with few flights do not get extreme weights.
- Ran weighted PageRank on each graph to get a **morning propagation risk** and an **evening accumulation risk** for every airport.
- The difference between the two scores sorts airports into "pure propagators" and "pure accumulators".

<figure>
  <img src="{{ '/assets/img/projects/airport_network.jpg' | relative_url }}" alt="Morning propagator and evening accumulator networks" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Left: morning departure network, colored by propagation risk. Right: evening arrival network, colored by accumulation risk.</figcaption>
</figure>

## Findings

- Large hubs carry the most risk in both graphs (DEN, DFW, ORD, ATL, CLT).
- **Propagators:** big hubs that scatter flights from the center (DEN, DFW, MIA), and leisure airports such as St. Thomas, Eagle County, and Honolulu. These look quiet, but return trips with families and heavy baggage seem to start delays.
- **Accumulators:** Anchorage, Atlanta, Las Vegas, and several eastern airports, where many afternoon flights arrive as final destinations because of time zones.
- The same hub can play a different role at different times of day, which a single per-airport delay rate hides.

<figure>
  <img src="{{ '/assets/img/projects/airport_roles.jpg' | relative_url }}" alt="Propagator versus accumulator scatter plot" style="width: 100%; max-width: 560px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Each dot is an airport (382 in total). Red: top 10 propagators. Blue: top 10 accumulators.</figcaption>
</figure>

The PageRank scores were then added as features to a gradient-boosted classifier, alongside time, airline, and aircraft features.

## Recommendations

- **Propagator airports:** put check-in and security staff on the morning peak, and use incentives for early and late flights to spread traffic.
- **Accumulator airports:** add baggage and ground crews and public transport in the afternoon, and turn simple arrivals into transfer connections.
