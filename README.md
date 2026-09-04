# CP-12 Compás Processor

<img width="50%" height="50%" alt="cp12-screenshot (1)" src="https://github.com/user-attachments/assets/aafa0de4-783a-4805-8aa8-e7fd71dd42e2" />

## Table of contents

1. [Introduction](#1-introduction)
2. [Master controls & architecture](#2-master-controls--architecture)
3. [Radial channels & sound shaping](#3-radial-channels--sound-shaping)
4. [The dynamic grid & velocity states](#4-the-dynamic-grid--velocity-states)
5. [The Duende (Improv) engine](#5-the-duende-improv-engine)
6. [Technical specifications](#6-technical-specifications)
7. [Credits & copyright](#7-credits--copyright)

## 1. Introduction

The CP-12 is not a standard linear step sequencer; it is a dynamic radial polyrhythm engine dedicated to the foundational rhythms of flamenco. Where traditional drum machines lock into rigid grids, the CP-12 fluidly adapts its architecture to generate the true *compás* of several distinct *palos*, as well as purely mathematical polyrhythmic ratios. It pits a heavy foot pattern against intersecting hand claps (*palmas*). The radial interface visualises the cycle as a continuous loop, mathematically rebuilding itself in real time to represent complex 12-step ternary polyrhythms, 8-step binary grooves, and tight 6-step *medio compás* cycles.

## 2. Master controls & architecture

The header panel governs global transport, pattern selection, and tempo.

### 2.1 Transport

| Control | Function |
|---|---|
| CLOCK / PLAY | A tactile rocker switch that toggles the global sequencer on and off. Engages the sub-millisecond lookahead scheduling engine on first press, and its adjacent status LED flares bright red while the master clock is running. |
| DUENDE (IMPROV) | A secondary tactile rocker switch that enables generative algorithmic syncopation, injecting fills, redobles, and drops into the active pattern without losing the underlying *compás*. |

### 2.2 Pattern Select

| Control | Function |
|---|---|
| PATTERN SELECT | A bank of nine LED-indicated tactile buttons to instantly switch the active pattern memory. Choosing a pattern dynamically reconstructs the radial UI and underlying sequence. Supported modes include pure mathematical generation (Poly 3:2), standard ternary *palos* (Bulería 12, Soleá, Alegrías, Seguiriyas, Fandangos), tight 6-step *medio compás* (Bulería 6), and 8-step binary grooves (Tangos, Rumbas). |

### 2.3 Tempo

| Control | Function |
|---|---|
| TEMPO (rotary) | A continuous rotary encoder controlling the master clock speed, scaling smoothly from a dragging 60 BPM up to a frantic 200 BPM. Drag vertically to adjust; the current value is shown live beneath the knob. |

## 3. Radial channels & sound shaping

The CP-12 is divided into three independent synthesiser voices, arranged as concentric rings: **FOOT** (inner, carmine red), **RIGHT HAND** (middle, brass), and **LEFT HAND** (outer, terracotta). Each ring carries its own dynamic pattern and colour-coded LEDs.

### 3.1 Channel routing

| Ring | Voice | Description |
|---|---|---|
| Inner | Foot (*Golpe/Tacón*) | A heavy, pitched-down oscillator thud. Anchors the groove with a driving low-end pulse. |
| Middle | Right Hand (*Palma Seca*) | A sharp, band-pass filtered dry clap representing the crack of fingers against the palm. Provides sharp, high-frequency syncopations. |
| Outer | Left Hand (*Palma Sorda*) | A resonant, low-pass filtered muffled clap. Creates a darker, rounded transient to establish the core rhythmic friction against the inner two rings. |

### 3.2 Synthesis parameters

Unlike a fader-per-track design, the CP-12 keeps its analogue voicing fixed in the sound engine rather than exposed on the panel — the emphasis is on rhythmic placement and velocity, not timbral sculpting. Each voice responds dynamically to the velocity of the step that triggers it:

| Parameter | Normal (State 1) | Accent (State 2) |
|---|---|---|
| Foot start frequency | 120 Hz | 180 Hz |
| Foot / Palma peak gain | Lower | Full |
| Palma filter frequency | Narrower / lower | Wider / higher |

## 4. The dynamic grid & velocity states

The sequencer matrix is arranged as three concentric rings of tactile, physical-style pads rather than a linear row. 

### 4.1 Step interactions & responsiveness

- **Dynamic Geometry** — The grid mathematically scales and reconstructs itself based on the selected pattern, forming a 12-step circle for standard ternary rhythms, an 8-step circle for binary rhythms (Tangos, Rumbas), or a 6-step circle for *medio compás* (Bulería 6).
- **Cycle & toggle** — Clicking or tapping a step's pad cycles it through three velocity states in sequence: Rest → Normal → Accent → Rest.
- **Visual feedback** — The embedded LED on each pad reflects its current state at rest (dark, muted glow, or bright flare with illuminated border), and briefly flashes white as the playhead strikes it during playback. The central silkscreen text updates dynamically to reflect the current cycle length.

### 4.2 Velocity states

The CP-12 uses a discrete velocity system in place of full parameter locking, which dynamically expands when the Duende engine is active:

| State | Appearance | Behaviour |
|---|---|---|
| 0 — Rest | Dark, flush with the chassis | Step is silent. |
| 0.5 — Ghost | Node flashes cyan | Triggered algorithmically by the Duende engine. Lower frequency and VCA peak (0.15–0.2 gain) for a subtle background strike. |
| 1 — Normal | Muted LED glow | Triggers a standard, controlled strike. |
| 2 — Accent | Bright LED flare, illuminated border | Triggers a hard strike — higher VCA peak and a wider filter cutoff for a sharper, brighter transient. |

## 5. The Duende (Improv) engine

When active, the Duende engine injects authentic flamenco syncopation into the *compás*. These generative events are visualised dynamically on the sequencer's central dashed guide rings.

* **Ghost Fills:** Has a 15% chance to fill completely empty steps with a subtle foot tap or muted clap. The central guide rings pulse with a watery cyan glow.
* **Syncopated Drops:** Has a 10% chance to drop an active beat entirely to create structural voids. The central guide rings completely dim out.
* **Redobles (Double Strikes):** Has a 15% chance to trigger a rapid double-tap (flam). The central guide rings flash a bright, resonant gold, and the step node itself flashes gold.

## 6. Technical specifications

- **Sequencer architecture**: Dynamic radial cycle adapting to 6, 8, or 12 steps; mathematically reconstructs geometry (x/y coordinates mapped via percentages for flawless responsive scaling) based on the selected pattern. Includes memory banks for Poly 3:2, Bulería 12, Bulería 6, Soleá, Alegrías, Seguiriyas, Tangos, Fandangos, and Rumbas.
- **Audio engine**: Pure Web Audio API synthesis, driven by a standard lookahead scheduler (25 ms tick, 100 ms schedule-ahead window) referenced against `audioCtx.currentTime` for sample-accurate timing independent of UI thread jitter.
- **Drum synthesis models**:
  - **Foot**: Triangle wave oscillator with a rapid exponential pitch drop into an exponential gain decay; start frequency and peak gain scaled by velocity (including the 0.5 Ghost state).
  - **Right Hand (Seca)**: Band-pass filtered white noise burst, tuned brighter and wider on accent, with slight randomised detuning ("humanise") to avoid mechanically identical hits.
  - **Left Hand (Sorda)**: Low-pass filtered white noise burst, tuned darker and rounder than the right-hand voice.
- **Algorithmic modulation (Duende)**: Introduces a non-destructive 0.5 velocity state for ghost notes and calculates a fixed 35ms humanised flam offset for redoble grace notes to prevent audio clipping and mechanical stuttering.
- **Interface**: Responsive, hardware-styled radial UI within a rack-mounted chassis. Uses Pointer Event handling for the tempo knob, click/tap handling for step nodes, and dynamic DOM rendering with CSS-driven dashed ring visualisers to seamlessly transition between time signatures.

## 7. Credits & copyright

CP-12 Compás Processor  
Created & Developed by José Velázquez MA  
Published by Voltage & Wave  
Website: [voltageandwave.co.uk](https://voltageandwave.co.uk/)

Copyright © 2026 José Velázquez MA / Voltage & Wave. All rights reserved.

