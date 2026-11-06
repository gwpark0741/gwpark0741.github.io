---
layout: page
title: Assistive Board-Game Robot
description: SO-101 arm with cameras and tactile sensing that moves pieces for players with limited hand mobility. In progress.
importance: 2
meta: "Sep 2026 – present · Creative Convergence Research Program"
img: /assets/img/projects/2_board_game_robot.jpg
category: research
---

<link rel="stylesheet" href="{{ '/assets/project-page.css' | relative_url }}">

**Sep 2026 – present · Team lead · Funded by Sogang's Creative Convergence Research Program (KRW 1.2M)**

_Status: hardware designed and parts ordered (Oct 2026); 3D printing and assembly are next. This page will be updated as the work progresses._

## Why

Players with limited hand or arm mobility can decide their own moves but still need someone to move the pieces. Digital board games remove the shared table. We want the player to keep making every decision while a robot does the physical part.

A board game is also a good first test for an assistive robot. The space is fixed, the objects are clear, and the actions repeat.

## Plan

<figure>
  <img src="{{ '/assets/img/projects/robot_plan.jpg' | relative_url }}" alt="Research flow" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Research flow: setup and calibration, real and simulated data collection, dataset building, VLA fine-tuning, RL, sim-to-real, and evaluation on a real board.</figcaption>
</figure>

1. **Hardware.** Assemble a SO-101 follower arm and a leader arm for teleoperation. Some parts are 3D-printed.
2. **Sensing.** A top camera sees the whole board and calibrates coordinates. A wrist camera guides the approach. Tactile sensors on the gripper check the grasp and detect drops.
3. **Simulation.** Rebuild the arm, board, and pieces at real scale in Isaac Sim.
4. **Data.** Record teleoperated demonstrations: language command, camera video, joint state, gripper state, and action.
5. **Learning.** Fine-tune a pretrained vision-language-action model on the demonstrations, then improve it with RL in Isaac Lab. The reward is staged: approach, grasp, lift, move, place.
6. **Sim-to-real.** Run on the real arm, log failures, collect more real data, and tune simulation parameters.
7. **Voice interface.** Speech to text, then the VLA, with a rule check for illegal moves.
8. **Evaluation.** Grasp, placement, and full-task success rates, time per move, positions not seen in training, and user feedback.

## Hardware design

I designed the full parts list and wiring before ordering, comparing an overseas and a domestic bill of materials.

- **Leader and follower arms.** The follower uses high-reduction (1:345) servos on all six joints for holding torque. The leader, which a person moves by hand, mixes gear ratios per joint (1:345, 1:191, 1:147) so it stays light to move.
- **Cameras.** A top camera on the official mount and a wrist camera, both through a powered USB hub.
- **Tactile sensing.** Two FSR402 force sensors on the inner faces of the gripper. Each forms a voltage divider with a 10 kΩ resistor, read by an ESP32 on two ADC pins and sent to the PC. I checked the ESP32 ADC range in advance, since accuracy drops above about 2.45 V.
- **Power and data.** Separate motor power for each arm, and one USB hub for both motor controllers, both cameras, and the ESP32.

<figure>
  <img src="{{ '/assets/img/projects/robot_layout.jpg' | relative_url }}" alt="SO-101 parts layout and wiring" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Parts layout: leader and follower arms, top and wrist cameras, gripper FSRs, and power and USB connections.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/robot_fsr.jpg' | relative_url }}" alt="FSR voltage divider circuit with ESP32" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Tactile sensing circuit: two FSRs with 10 kΩ divider resistors into ESP32 GPIO34 and GPIO35, and the breadboard layout.</figcaption>
</figure>

## Simulation

<figure>
  <img src="{{ '/assets/img/projects/robot_isaacsim.jpg' | relative_url }}" alt="SO-101 in Isaac Sim" style="width: 100%; max-width: 560px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">The SO-101 asset loaded in Isaac Sim.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/robot_isaaclab.jpg' | relative_url }}" alt="Parallel RL environments in Isaac Lab" style="width: 100%; max-width: 560px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Parallel reinforcement learning environments in Isaac Lab, the setup we plan to use for the RL stage.</figcaption>
</figure>
