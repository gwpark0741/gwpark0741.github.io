---
layout: page
title: Assistive Board-Game Robot
description: SO-101 arm with cameras and tactile sensing that moves pieces for players with limited hand mobility. In progress.
importance: 3
meta: "Sep 2026 – present · Creative Convergence Research Program"
img: /assets/img/projects/2_board_game_robot.jpg
category: research
---

<link rel="stylesheet" href="{{ '/assets/project-page.css' | relative_url }}">

<table class="project-info">
  <tr><th>Period</th><td>Sep 2026 – present</td></tr>
  <tr><th>Context</th><td>Creative Convergence Research Program (Sogang University), KRW 1.2M funding</td></tr>
  <tr><th>Role</th><td>Team lead</td></tr>
  <tr><th>Outcome</th><td>In progress</td></tr>
</table>

_Status: parts ordered (Oct 2026), arm parts 3D-printed and both arms assembled (Nov 2026). Teleoperation works (Nov 2026) and the simulation environment is running; next is swapping in a camera-mount gripper and starting data collection. This page will be updated as the work progresses._

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

## 3D printing

The structural parts of both arms are 3D-printed. The workflow was: download the parts from the official [SO-101 repository](https://github.com/TheRobotStudio/SO-ARM100), arrange them on build plates in Bambu Studio (three plates for the follower and three for the leader), slice them, and print on a Bambu Lab A1 mini in eSUN PLA+ (0.4 mm nozzle, 0.2 mm layers, 15% infill, 2 walls).

<figure>
  <img src="{{ '/assets/img/projects/robot_print_plates.jpg' | relative_url }}" alt="Print plates for the follower and leader arms" style="width: 100%;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Print plates in Bambu Studio. Top row: follower. Bottom row: leader.</figcaption>
</figure>

<figure>
  <video controls muted loop playsinline preload="metadata" poster="{{ '/assets/img/projects/robot_printing_poster.jpg' | relative_url }}" style="width: 100%; max-width: 340px;">
    <source src="{{ '/assets/video/so101-printing.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Printing one of the arm plates (Nov 2026).</figcaption>
</figure>

Once both arms were calibrated, I tested teleoperation: moving the leader by hand makes the follower copy the motion in real time. This is how the demonstrations for the VLA will be recorded.

<figure>
  <video controls muted loop playsinline preload="metadata" poster="{{ '/assets/img/projects/robot_teleop_poster.jpg' | relative_url }}" style="width: 100%; max-width: 340px;">
    <source src="{{ '/assets/video/so101-teleop.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Teleoperation test (Nov 2026): the follower arm (back) follows the leader arm (front) moved by hand.</figcaption>
</figure>

Print files, sliced for the A1 mini: [Follower (3mf)]({{ '/assets/files/so101/BambuLabA1mini_Follower_SO101.3mf' | relative_url }}) · [Leader (3mf)]({{ '/assets/files/so101/BambuLabA1mini_Leader_SO101.3mf' | relative_url }})

## Assembly

I assembled both arms from the printed parts and the servos. The servos are wired in a daisy chain per arm, and each arm connects to the PC through the powered USB hub.

<figure>
  <img src="{{ '/assets/img/projects/robot_unboxing.jpg' | relative_url }}" alt="Printed parts and servo boxes" style="width: 100%; max-width: 300px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Printed parts and servo boxes, before assembly.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/robot_assembled.jpg' | relative_url }}" alt="Assembled leader and follower arms" style="width: 100%; max-width: 560px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">The assembled leader and follower arms, connected to the PC through the USB hub (Nov 2026).</figcaption>
</figure>

## Troubleshooting: teleoperation kept cutting out

**Symptom.** With the first teleoperation tests, the follower arm was weak and the connection kept dropping, each time with a warning beep.

**Diagnosis.**

1. The follower's servos are 12 V, but I was powering it from a 5 V / 5 A adapter.
2. I measured the supply voltage while operating the arm. It stayed at only 4–5 V instead of 12 V.
3. So the weak torque and the dropouts came from undervoltage, not from software or calibration.

**Fix.** I bought a 12 V / 5 A adapter and connected it to the follower. Teleoperation then ran without dropouts.

**Lesson.** Check the rated voltage of each arm's servos against its supply before debugging software, and measure the voltage under load, not just at idle.

## Next: wrist camera mount

The stock gripper has no place for the wrist camera. I am replacing the follower's gripper with a camera-integrated version and printing it on the A1 mini. After that, the wrist camera goes on and the arm is ready to record demonstrations.

## Simulation

I compared simulators for this project against six questions: does it ship an SO-101 model, does it save data in LeRobot format (so it can feed VLA training directly), can a leader arm drive it for extra demonstrations, does it run on a 24 GB RTX A5000, how hard is it to add a chess board and pieces, and has anyone done sim-to-real with it.

I chose [LeIsaac](https://lightwheelai.github.io/leisaac/), an Isaac Lab extension for SO-101. It includes the follower arm, accepts a leader arm (or keyboard, gamepad) for teleoperation, converts recorded HDF5 demonstrations to LeRobot format, and can generate data automatically without a human operator. The chess environment is built on top of it: the board is generated in code (64 squares, adjustable size) and the pieces are low-poly USD models reused from the SO101-Chess repository. It renders on the A5000 server with the top and wrist cameras.

<figure>
  <video controls muted loop playsinline preload="metadata" poster="{{ '/assets/img/projects/leisaac_chess_poster.jpg' | relative_url }}" style="width: 100%; max-width: 640px;">
    <source src="{{ '/assets/video/leisaac-chess.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Our chess environment: wrist camera (left) and top camera (right). The arm only waves its joints to check that rendering works; this is not a learned policy.</figcaption>
</figure>

<figure>
  <video controls muted loop playsinline preload="metadata" poster="{{ '/assets/img/projects/leisaac_pickorange_poster.jpg' | relative_url }}" style="width: 100%; max-width: 640px;">
    <source src="{{ '/assets/video/leisaac-pickorange.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">LeIsaac's built-in kitchen example (PickOrange), run on the same server, with the same test motion.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/robot_isaacsim.jpg' | relative_url }}" alt="SO-101 in Isaac Sim" style="width: 100%; max-width: 560px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">The SO-101 asset loaded in Isaac Sim.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/projects/robot_isaaclab.jpg' | relative_url }}" alt="Parallel RL environments in Isaac Lab" style="width: 100%; max-width: 560px;">
  <figcaption style="font-size: 0.8rem; opacity: 0.7;">Parallel reinforcement learning environments in Isaac Lab, the setup we plan to use for the RL stage.</figcaption>
</figure>
