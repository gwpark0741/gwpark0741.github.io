---
layout: page
title: Audyssey
description: A multi-track agent that splits a video's audio into one track per sound source, generates missing sounds on request, and edits each track from a text command. Work in progress; related to my KSC 2026 paper.
importance: 1
meta: "Fall 2026 · Capstone Design · In progress"
img: /assets/img/projects/audyssey_tracks.jpg
category: research
---

<link rel="stylesheet" href="{{ '/assets/project-page.css' | relative_url }}">

**Fall 2026 · Capstone Design · In progress · My role: system architecture and integration, Planner/Validator, Separator quality gate, Router**

A five-person team project. Poster submitted to the 2026 Sogang Capstone Design Competition (final round on Dec 9, 2026). This page covers my parts only. I set the designs, reviewed and approved the results, and implemented them with AI coding agents (Claude Code).

## Problem

Current video-to-audio models return one mixed track, so you cannot turn one sound down or fix one sound. Audyssey takes a video of up to 30 seconds, separates its audio into one track per sound source, generates sounds the video does not contain when the user asks for them, and exports the edited video and per-track audio. A silent video gets generated sound only (no speech).

## Pipeline

<figure>
  <div style="display: flex; flex-wrap: wrap; gap: 0.4rem 0.5rem; align-items: center; font-size: 0.8rem;">
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Video</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Planner</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Validator</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Separator / Generator</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Reviewer</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Multi-track mix</span>
  </div>
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">The Validator can send a plan back once, and the Reviewer can send a track back up to three times. The Router turns a text command into one of eight edit commands on the mix.</figcaption>
</figure>

| Module    | What it does                                                                                                                                                                                                                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Planner   | Reads the video and its audio (Gemini, 2 frames per second) and lists the sound sources with a type, a description, and time spans.                                                                                             |
| Validator | A different model (Qwen3-VL-8B, video only, 5 frames per second) checks each source against the video and rejects sources with no evidence. Sources that pass are kept, and only the rest are planned again (at most 2 rounds). |
| Separator | Splits one source out of the original audio with SAM Audio, ranking several candidates per sound.                                                                                                                               |
| Generator | Makes sounds that are not in the video from the video and a text description, using an in-house generation model our team developed (paper in preparation).                                                                     |
| Reviewer  | Scores each separated track with the SAM Audio Judge, and each generated track with a CLAP-based margin. A track that misses the bar is retried up to 3 times.                                                                  |
| Router    | Turns "bgm half as loud" into one of 8 edit commands, or asks back when the request is unclear or cannot be done.                                                                                                               |

I wrote the Planner and Validator (PRs #8, #28, #41), the Separator and Judge setup (#13, #18, #35, #52), the Router (#38, #39), and the generation path that connects the in-house model to the pipeline (#23 to #25). I also built the server split, the Docker images, and the CI/CD.

## Result

<figure>
  <img src="{{ '/assets/img/projects/audyssey_tracks.jpg' | relative_url }}" alt="Audyssey editor showing the pipeline status, the text edit bar, and one waveform track per sound source" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Editor after one run: pipeline status, the text edit bar, and one track per source (music, dialogue, shouts, sword sounds) plus the rest of the original audio. The video frame is cropped out.</figcaption>
</figure>

In one demo run on a 9.8-second animated fight clip, the Planner proposed 5 sources and the Validator rejected one that had no visual evidence (a body strike at 7.7 to 8.3 s). The 4 remaining sources were separated, and the Judge passed all 4 on the first try (overall 3.83 to 4.76). The whole run took about 277 seconds, including model loading. This is a single clip, not a benchmark.

## Judge pass bar: 0.5 to 3.0

The default pass threshold of the SAM Audio Judge was 0.5, below the 1 to 5 score range, so nothing was ever rejected and no retry ever ran. Changing a track's description to "church bells ringing", which is not in the video, still scored 1.80 and passed.

I measured real scores on that clip. Correct tracks scored 3.83 to 4.76 (5 cases), and the same track with the wrong description scored 1.80, 1.58, and 1.30 over three tries. I set the bar to 3.0, between the two groups (PR #52).

<figure>
  <img src="{{ '/assets/img/projects/audyssey_judge_fail.jpg' | relative_url }}" alt="Audyssey track panel showing a failed quality check: judge overall 1.30 below 3.0 for the description church bells ringing" style="width: 100%; max-width: 360px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">With a description that is not in the video, the track fails after three attempts (judge overall 1.30 < 3.0).</figcaption>
</figure>

Limits: this is one video, and no person listened to the tracks to label them. A wrong description of "a baby crying loudly" still scored 4.18 and passed, so the Judge score alone does not catch every wrong sound.

## Router: text to edit commands

| Setup                                                                                      | macro-F1 (dev) |
| ------------------------------------------------------------------------------------------ | -------------- |
| Keyword rules                                                                              | 0.185          |
| Small local model alone (Qwen2.5-3B)                                                       | 0.553          |
| Final: decision model (H2O-Lightning-4B) plus Qwen2.5-3B for values, plus a rule guardrail | 0.906          |

The final setup answers in 81 ms (median, local vLLM, hardware not recorded) and scored 0.846 on the held-out test set. Gemini scores higher (0.975 or more on dev) but takes about 2.5 seconds per reply. The evaluation set has 238 requests written by Claude, Codex, and Gemini, 30 of which a person checked. Conditions: 73 dev requests, 5 runs each, split by request template, 95% interval about 0.15 wide, so 0.892 and 0.906 are not statistically different. The demo uses Gemini to fill in values, so these scores are for the research setup, not for the demo.

## Related paper: KSC 2026

An audio language model that checks tracks must answer "is any sound other than the target audible?" correctly. Qwen2.5-Omni 7B answered "is a refrigerator audible?" correctly for an alarm clip but got "is any sound other than a refrigerator audible?" wrong, which would let a track without its target sound pass. My first-author KSC 2026 paper (poster, accepted; listed on the About page) analyzes why models ignore the condition in these exclusion questions and fixes it with counterfactual training data.

- Pair accuracy (both versions of a question answered correctly) rose from 5.2, 24.8, and 56.2% to 83.5, 83.9, and 84.5% for Qwen2.5-Omni 3B, 7B, and Qwen3-Omni 30B-A3B.
- General audio understanding (MMAU test-mini) did not drop significantly.
- Conditions: 220 human-checked separated tracks with 3 question pairs each (660 pairs), LoRA training on 4,800 examples for 150 steps, mean of 3 seeds. The paper also reports that errors on correct tracks rose (7B: 3.6 to 13.2%).

This trained model is not part of the Audyssey pipeline yet. The system's Reviewer uses the Judge and CLAP scores above.

## System and engineering

The GPU models (SAM Audio, the Judge, SAM 3, the generation model, CLAP) run on a GPU tools server. The app server has no GPU and calls it over HTTP with a token. The split was needed because SAM Audio's dependencies require `numpy<2`, which conflicts with the rest of the app. Each tool can run as a stub, locally, or remotely, so a teammate can run only their own module with a real model. A demo run fits on one A100 80GB (peak 71,638 MiB).

- Image deployment after changing only `src/`: 650 s to 7 s, measured with a warm local registry cache. On GitHub runners it took 9 to 12 minutes before.
- Model loading: the first Judge call took 32.6 s, and later calls about 0.3 s per source (one run).
- Tests: 455 unit tests at PR #52. Eight edit commands go through the same checks whether they come from a button, the Router, or an agent.

## Limits and next steps

Only one video was used for the Judge bar and the demo, and the generated-sound check (CLAP margin 0.0) has not been calibrated. Sources are processed one by one on a single server process, with no job queue or user accounts, so I make no claim about scale or time saved. Next steps are listening-based labels for the Judge and the generated-sound check, running the KSC model as the Reviewer, and publishing the in-house generation model.
