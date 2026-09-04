# CP-12 Compás Processor

<img width="870" height="816" alt="cp12-screenshot" src="https://github.com/user-attachments/assets/83113677-13bc-439d-9257-8d2ab3624539" />

## Table of contents

1. [Introduction](#1-introduction)
2. [Master controls & architecture](#2-master-controls--architecture)
3. [Radial channels & sound shaping](#3-radial-channels--sound-shaping)
4. [The 12-step grid & velocity states](#4-the-12-step-grid--velocity-states)
5. [Technical specifications](#5-technical-specifications)
6. [Credits & copyright](#6-credits--copyright)

## 1. Introduction

The CP-12 is not a standard linear step sequencer; it is a radial polyrhythm engine dedicated to the *compás*—the 12-beat metric cycle foundational to flamenco. Specifically, it maps the driving tension of the bulería. Where traditional drum machines lock into rigid 4/4 grids, the CP-12 generates groove through a 3-against-2 polyrhythm. It pits a heavy 3-beat foot pattern against intersecting 2-beat and 3-beat hand claps (*palmas*). The radial interface visualizes this 12-step cycle as a continuous loop, geometrically mapping exactly where the duple and triple meters collide.


## 2. Master controls & architecture

The header panel governs global transport and tempo.

### 2.1 Transport

| Control | Function |
|---|---|
| CLOCK / PLAY | A tactile rocker switch that toggles the global sequencer on and off. Engages the sub-millisecond lookahead scheduling engine on first press, and its adjacent status LED flares bright red while the master clock is running. |

### 2.2 Tempo

| Control | Function |
|---|---|
| TEMPO (rotary) | A continuous rotary encoder controlling the master clock speed, scaling smoothly from a dragging 60 BPM up to a frantic 200 BPM. Drag vertically to adjust; the current value is shown live beneath the knob. |

## 3. Radial channels & sound shaping

The CP-12 is divided into three independent synthesiser voices, arranged as concentric rings: **FOOT** (inner, carmine red), **RIGHT HAND** (middle, brass), and **LEFT HAND** (outer, terracotta). Each ring carries its own fixed 12-step pattern and its own colour-coded LEDs.

### 3.1 Channel routing

| Ring | Voice | Description |
|---|---|---|
| Inner | Foot (*Golpe/Tacón*) | A heavy, pitched-down oscillator thud. Anchors the groove; defaults to a 3-beat subdivision with a hard accent on beat 1. |
| Middle | Right Hand (*Palma Seca*) | A sharp, band-pass filtered dry clap representing the crack of fingers against the palm. Locks into the 3-beat subdivision alongside the foot. |
| Outer | Left Hand (*Palma Sorda*) | A resonant, low-pass filtered muffled clap. Runs on a 2-beat subdivision, creating the core polyrhythmic friction against the inner two rings. |

### 3.2 Synthesis parameters

Unlike a fader-per-track design, the CP-12 keeps its analogue voicing fixed in the sound engine rather than exposed on the panel — the emphasis is on rhythmic placement and velocity, not timbral sculpting. Each voice does, however, respond dynamically to the velocity of the step that triggers it:

| Parameter | Normal (State 1) | Accent (State 2) |
|---|---|---|
| Foot start frequency | 120 Hz | 180 Hz |
| Foot / Palma peak gain | Lower | Full |
| Palma filter frequency | Narrower / lower | Wider / higher |

## 4. The 12-step grid & velocity states

The sequencer matrix is arranged as three concentric rings of tactile, physical-style pads rather than a linear row, so that the 3:2 ratio between the hand and foot tracks is visible as a geometric pattern around the circle.

### 4.1 Step interactions

- **Cycle & toggle** — clicking or tapping a step's pad cycles it through three velocity states in sequence: Rest → Normal → Accent → Rest.
- **Visual feedback** — the embedded LED on each pad reflects its current state at rest (dark, muted glow, or bright flare with illuminated border), and briefly flashes white as the playhead strikes it during playback.

### 4.2 Velocity states

The CP-12 uses a three-state velocity system in place of full parameter locking:

| State | Appearance | Behaviour |
|---|---|---|
| 0 — Rest | Dark, flush with the chassis | Step is silent. |
| 1 — Normal | Muted LED glow | Triggers a standard, controlled strike. |
| 2 — Accent | Bright LED flare, illuminated border | Triggers a hard strike — higher VCA peak and a wider filter cutoff for a sharper, brighter transient. |

## 5. Technical specifications

- **Sequencer architecture**: 3 independent voices sharing a single 12-step radial cycle; a fixed 3-beat (Foot, Right Hand) and 2-beat (Left Hand) subdivision generates the underlying 3:2 polyrhythm.
- **Audio engine**: Pure Web Audio API synthesis, driven by a standard lookahead scheduler (25 ms tick, 100 ms schedule-ahead window) referenced against `audioCtx.currentTime` for sample-accurate timing independent of UI thread jitter.
- **Drum synthesis models**:
  - **Foot**: Triangle wave oscillator with a rapid exponential pitch drop into an exponential gain decay; start frequency and peak gain scaled by velocity.
  - **Right Hand (Seca)**: Band-pass filtered white noise burst, tuned brighter and wider on accent, with slight randomised detuning ("humanise") to avoid mechanically identical hits.
  - **Left Hand (Sorda)**: Low-pass filtered white noise burst, tuned darker and rounder than the right-hand voice.
- **Velocity modulation**: Three-state (Rest / Normal / Accent) array-based pattern data per voice, 12 steps × 3 tracks.
- **Interface**: Hardware-styled 340px radial UI within a rack-mounted chassis, using Pointer Event handling for the tempo knob and click/tap handling for step nodes.

## 6. Credits & copyright

CP-12 Compás Processor
Created & Developed by José Velázquez MA
Published by Voltage & Wave
Website: [voltageandwave.co.uk](https://voltageandwave.co.uk/)

Copyright © 2026 José Velázquez MA / Voltage & Wave. All rights reserved.

---

### About

A radial 3:2 polyrhythm generator bridging the 12-beat flamenco *compás* with the Web Audio API.
