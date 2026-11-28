---
layout: page
title: SoundPilot
description: A tool-calling agent that turns a silent video into editable multi-track sound. Capstone Gold Prize (1st place), KCC 2026 accepted, AI·SW competition Sponsor Company Award (Elice Group).
importance: 1
meta: "Spring 2026 · Capstone Design"
img: /assets/img/projects/1_soundpilot.jpg
category: research
---

<link rel="stylesheet" href="{{ '/assets/project-page.css' | relative_url }}">

**Spring 2026 · Capstone Design · My role: problem definition and agent pipeline design**

A tool-calling agent that turns a silent video into editable multi-track sound. Each sound is generated on its own track, routed to the model that fits it, and grounded in explicit visual evidence.

| Recognition            | Detail                                                                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Gold Prize (1st place) | Sogang Capstone Design Competition, June 2026                                                                                               |
| KCC 2026               | Accepted, first author ([DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE12930030))                                         |
| Sponsor Company Award  | Elice Group, at the AI·SW-Centered University Excellent Project Competition, as Sogang's representative (18 of 60 teams reached the finals) |

Full materials (Korean): [AI·SW competition slides (PDF)]({{ '/assets/slides/soundpilot-agent.pdf' | relative_url }}) · [Capstone poster (PDF)]({{ '/assets/slides/soundpilot-poster.pdf' | relative_url }}) · [KCC 2026 poster (PDF)]({{ '/assets/slides/kcc-poster.pdf' | relative_url }})

## Problem

Current sound generation models have three limits.

- All sounds come out as one mixed waveform, so you cannot edit just one of them.
- Summarizing the video as text loses fine motion, so timing drifts.
- The model gives no clear evidence for why a sound is there.

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s3.jpg' | relative_url }}" alt="Limits of existing models and the target system" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Limits of existing sound generation models and what SoundPilot targets: separate editing, correct timing, and visual grounding.</figcaption>
</figure>

## Pipeline

<figure>
  <div style="display: flex; flex-wrap: wrap; gap: 0.4rem 0.5rem; align-items: center; font-size: 0.8rem;">
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Video</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Video analysis</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Tool-calling agents</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Model routing</span> →
    <span style="border: 1px solid; border-radius: 6px; padding: 0.2rem 0.55rem;">Editable sound timeline</span>
  </div>
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">One agentic workflow from video analysis to sound planning, generation, and editing.</figcaption>
</figure>

| Step             | What it does                                                                                                                                                                                                                                                                                                                                  |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Video analysis   | Splits the video into scenes and samples key frames (at least one every 2 seconds). A VLM lists the sound-making objects, SAM 3 tracks them, and changes in their masks (appearing, overlapping, moving) become event candidates. Everything is stored in a form the agents can query.                                                        |
| Agents           | Agents read scene, visual, and object information only through tools, and keep querying nearby frames until they have enough evidence. Four workers run in parallel on a shared memory. Before adding an event, each one checks whether the same sound already has a track and reuses it, so a sound stays consistent across the whole video. |
| Model routing    | Action sounds that must match motion go to video-to-audio (HunyuanVideo-Foley). Background sound and music go to text-to-audio (ElevenLabs). Speech goes to text-to-speech.                                                                                                                                                                   |
| Timeline editing | Users can play each track, add or remove events, move timing, and edit a description to regenerate one sound.                                                                                                                                                                                                                                 |

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s4.jpg' | relative_url }}" alt="Sound design agent system" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">System overview.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s5.jpg' | relative_url }}" alt="Video analysis pipeline" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Video analysis: scene split, key frames, object identification, tracking with SAM 3, event estimation, and structured storage.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s6.jpg' | relative_url }}" alt="Agent system design" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Agents: parallel tool-calling workers with a shared sound timeline.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s7.jpg' | relative_url }}" alt="Model routing by sound type" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Model routing: each sound type goes to the model that fits it.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s8.jpg' | relative_url }}" alt="Sound timeline editing" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Timeline editing: track playback, event editing, timing adjustment, and prompt-based regeneration.</figcaption>
</figure>

## Result

On a 17.2-second sword fight, SoundPilot produced 7 tracks and 36 events, with effects, voices, and background sound on separate tracks.

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s9.jpg' | relative_url }}" alt="SoundPilot UI" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Upload, plan, edit, and generate in one screen.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s10.jpg' | relative_url }}" alt="Result on a sword fight scene" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">The sword fight result: 7 tracks and 36 events.</figcaption>
</figure>

## Earlier version: KCC 2026 paper

The first version produced multi-track sound metadata (track, source, type, event type, timestamps, description) without the agent. A sound-source registry over the whole video kept repeated sounds on one track, where a commercial multi-track service split them at every cut.

| Setup                | Events (mean) | Tracks (mean) |
| -------------------- | ------------- | ------------- |
| With the registry    | 72.7          | 19.7          |
| Without the registry | 13.3          | 6.3           |

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_tracks.jpg' | relative_url }}" alt="Track fragmentation compared with our merged tracks" style="width: 100%; max-width: 640px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">(A) A commercial service splits the same sound into a new track at every cut. (B) Our registry keeps it on one track.</figcaption>
</figure>

## At the competition

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_award.jpg' | relative_url }}" alt="Receiving the Sponsor Company Award at the AI·SW-Centered University Excellent Project Competition" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Receiving the Sponsor Company Award (Elice Group) at the AI·SW-Centered University Excellent Project Competition.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_stage.jpg' | relative_url }}" alt="Presenting SoundPilot on stage" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Presenting SoundPilot on stage.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_booth.jpg' | relative_url }}" alt="Presenting SoundPilot at the poster booth" style="width: 100%; max-width: 420px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">At the poster booth.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_capstone_certificate.jpg' | relative_url }}" alt="Gold Prize certificate from the Sogang Capstone Design Competition" style="width: 100%; max-width: 420px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Gold Prize certificate, Sogang Capstone Design Competition (June 11, 2026).</figcaption>
</figure>

## Limits and next steps

There is no standard metric for multi-track sound yet, so evaluation is mostly qualitative, and VLM outputs still vary between runs.

Next steps are a quantitative and human evaluation protocol and more deterministic analysis to reduce reliance on the VLM.
