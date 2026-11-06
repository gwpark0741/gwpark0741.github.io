---
layout: page
title: SoundPilot
description: A tool-calling agent that turns a silent video into editable multi-track sound. Capstone Gold Prize (1st place), KCC 2026 accepted, AI·SW Main Award Finalist.
importance: 1
meta: "Spring 2026 · Capstone Design"
img: /assets/img/projects/1_soundpilot.jpg
category: research
---

<link rel="stylesheet" href="{{ '/assets/project-page.css' | relative_url }}">

**Spring 2026 · Capstone Design · My role: problem definition and agent pipeline design**

Gold Prize (1st place) at the Sogang Capstone Design Competition · KCC 2026 accepted, first author ([DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE12930030)) · Main Award Finalist at the AI·SW-Centered University Excellent Project Competition (Sogang's representative, 18 of 60 teams)

## Problem

Current sound generation models have three limits. All sounds come out as one mixed waveform, so you cannot edit just one of them. Summarizing the video as text loses fine motion, so timing drifts. And the model gives no clear evidence for why a sound is there. SoundPilot generates each sound on its own track, routes it to the right model, and grounds every sound in explicit visual evidence.

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s3.jpg' | relative_url }}" alt="Limits of existing models and the target system" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Limits of existing sound generation models and what SoundPilot targets: separate editing, correct timing, and visual grounding.</figcaption>
</figure>

## System

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s4.jpg' | relative_url }}" alt="Sound design agent system" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">One agentic workflow from video analysis to sound planning, generation, and editing.</figcaption>
</figure>

### 1. Video analysis

The video is split into scenes, key frames are sampled (at least one every 2 seconds), and a VLM lists the sound-making objects. SAM 3 tracks those objects, and changes in their masks (appearing, overlapping, moving) become event candidates. Everything is stored in a form the agents can query.

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s5.jpg' | relative_url }}" alt="Video analysis pipeline" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Scene split, key frames, object identification, tracking with SAM 3, event estimation, and structured storage.</figcaption>
</figure>

### 2. Agent design

Agents read scene, visual, and object information only through tools, and keep querying nearby frames until they have enough evidence. Four workers run in parallel on a shared memory. Before adding an event, each one checks whether the same sound already has a track and reuses it, so a sound stays consistent across the whole video.

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s6.jpg' | relative_url }}" alt="Agent system design" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Parallel tool-calling agents with a shared sound timeline.</figcaption>
</figure>

### 3. Model routing

Action sounds that must match motion go to video-to-audio (HunyuanVideo-Foley). Background sound and music go to text-to-audio (ElevenLabs). Speech goes to text-to-speech.

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s7.jpg' | relative_url }}" alt="Model routing by sound type" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Each sound type goes to the model that fits it.</figcaption>
</figure>

### 4. Editing the sound timeline

Users can play each track, add or remove events, move timing, and edit a description to regenerate one sound.

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s8.jpg' | relative_url }}" alt="Sound timeline editing" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Track playback, event editing, timing adjustment, and prompt-based regeneration.</figcaption>
</figure>

## Result

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s9.jpg' | relative_url }}" alt="SoundPilot UI" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Upload, plan, edit, and generate in one screen.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_s10.jpg' | relative_url }}" alt="Result on a sword fight scene" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">A 17.2-second sword fight: 7 tracks and 36 events, with effects, voices, and background sound on separate tracks.</figcaption>
</figure>

## Earlier version: KCC 2026 paper

The first version produced multi-track sound metadata (track, source, type, event type, timestamps, description) without the agent. A sound-source registry over the whole video kept repeated sounds on one track, where a commercial multi-track service split them at every cut. With the registry, the pipeline found 72.7 events and 19.7 tracks on average, versus 13.3 events and 6.3 tracks without it. We also measured that Gemini 2.5 Pro gave 10, 12, and 18 cuts on three runs of the same video, and switched to a deterministic scene detector.

<figure>
  <img src="{{ '/assets/img/projects/soundpilot_tracks.jpg' | relative_url }}" alt="Track fragmentation compared with our merged tracks" style="width: 100%; max-width: 640px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">(A) A commercial service splits the same sound into a new track at every cut. (B) Our registry keeps it on one track.</figcaption>
</figure>

## Limits and next steps

There is no standard metric for multi-track sound yet, so evaluation is mostly qualitative, and VLM outputs still vary between runs. Next steps are a quantitative and human evaluation protocol and more deterministic analysis to reduce reliance on the VLM.
