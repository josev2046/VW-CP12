# CP-12 *Compás* processor
[![DOI](https://zenodo.org/badge/1357103336.svg)](https://doi.org/10.5281/zenodo.22308834)

## Table of contents

1. [Introduction](#1-introduction)
2. [Quick Start](#2-quick-start)
3. [Master Controls & Architecture](#3-master-controls--architecture)
4. [Radial Channels & Sound Shaping](#4-radial-channels--sound-shaping)
5. [The Dynamic Grid & Velocity States](#5-the-dynamic-grid--velocity-states)
6. [The Duende (Improv) Engine](#6-the-duende-improv-engine)
7. [Pattern Memory Reference](#7-pattern-memory-reference)
8. [Technical Specifications](#8-technical-specifications)
9. [Troubleshooting](#9-troubleshooting)
10. [Credits & Copyright](#10-credits--copyright)

---

## 1. Introduction

The CP-12 is not a standard linear step sequencer; it is a dynamic radial polyrhythm engine dedicated to the foundational rhythms of flamenco. Where traditional drum machines lock into rigid grids, the CP-12 fluidly adapts its architecture to generate the true *compás* of several distinct *palos*, as well as a purely mathematical polyrhythmic ratio.

It pits a heavy foot pattern against intersecting hand claps (*palmas*). The radial interface visualises the cycle as a continuous loop, mathematically rebuilding itself in real time to represent complex 12-step ternary polyrhythms, 8-step binary grooves, and tight 6-step *medio compás* cycles.

<img width="1800" height="919" alt="signal_architecture" src="https://github.com/user-attachments/assets/466c427b-b665-47f2-a09e-bf50ec8a1914" />

---

## 2. Quick start

1. **Pick a pattern.** Choose one of the nine PATTERN SELECT buttons (defaults to BULERÍA 12 on load). The radial grid rebuilds itself instantly to match the new cycle length.
2. **Press CLOCK / PLAY.** The rocker switch lights up and the transport starts; the first press also initialises the audio engine, so playback begins a beat after the click.
3. **Program steps.** Click or tap any node on the three rings to cycle it through **Rest → Normal → Accent → Rest**.
4. **Set the tempo.** Drag the TEMPO knob vertically — up increases BPM, down decreases it — across a 60–200 BPM range.
5. **Add feel.** Engage DUENDE (IMPROV) to let the engine layer in ghost notes, drops, and redobles on top of your pattern without breaking the underlying *compás*.

---

## 3. Master controls & architecture

The header panel governs global transport, pattern selection, and tempo.

### 3.1 Transport

| Control | Function |
|---|---|
| **CLOCK / PLAY** | A tactile rocker switch that toggles the global sequencer on and off. Engages the sub-millisecond lookahead scheduling engine on first press, and its adjacent status LED flares bright red while the master clock is running. |
| **DUENDE (IMPROV)** | A secondary tactile rocker switch that enables generative algorithmic syncopation, injecting fills, redobles, and drops into the active pattern without losing the underlying *compás*. |

### 3.2 Pattern select

A bank of nine LED-indicated tactile buttons instantly switches the active pattern memory. Choosing a pattern dynamically reconstructs the radial UI and underlying sequence:

- Pure mathematical generation — **Poly 3:2**
- Standard ternary *palos* — **Bulería 12, Soleá, Alegrías, Seguiriyas, Fandangos**
- Tight 6-step *medio compás* — **Bulería 6**
- 8-step binary grooves — **Tangos, Rumbas**

### 3.3 Tempo

A continuous rotary encoder controls the master clock speed, scaling smoothly from a dragging **60 BPM** up to a frantic **200 BPM**. Drag vertically to adjust; the current value is shown live beneath the knob.

---

## 4. Radial channels & sound shaping

The CP-12 is divided into three independent synthesiser voices, arranged as concentric rings. Each ring carries its own dynamic pattern and colour-coded LEDs.

<img width="1100" height="1240" alt="radial_channel_map" src="https://github.com/user-attachments/assets/4c390613-3835-4ffc-8482-bbaf6fac9a56" />


### 4.1 Channel routing

| Ring | Voice | Description |
|---|---|---|
| Inner | **Foot** (*Golpe/Tacón*) | A heavy, pitched-down oscillator thud. Anchors the groove with a driving low-end pulse. |
| Middle | **Right hand** (*Palma seca*) | A sharp, band-pass filtered dry clap representing the crack of fingers against the palm. Provides sharp, high-frequency syncopations. |
| Outer | **Left hand** (*Palma sorda*) | A resonant, low-pass filtered muffled clap. Creates a darker, rounded transient that establishes the core rhythmic friction against the inner two rings. |

### 4.2 Synthesis parameters

Unlike a fader-per-track design, the CP-12 keeps its analogue voicing fixed in the sound engine rather than exposed on the panel — the emphasis is on rhythmic placement and velocity, not timbral sculpting. Each voice responds dynamically to the velocity of the step that triggers it:

| Parameter | Normal (State 1) | Accent (State 2) |
|---|---|---|
| Foot start frequency | 120 Hz | 180 Hz |
| Foot / Palma peak gain | Lower | Full |
| Palma filter frequency | Narrower / lower | Wider / higher |

---

## 5. The dynamic grid & velocity atates

The sequencer matrix is arranged as three concentric rings of tactile, physical-style pads rather than a linear row.

### 5.1 Step interactions & responsiveness

- **Dynamic geometry** — The grid mathematically scales and reconstructs itself based on the selected pattern, forming a 12-step circle for standard ternary rhythms, an 8-step circle for binary rhythms (Tangos, Rumbas), or a 6-step circle for *medio compás* (Bulería 6).
- **Cycle & toggle** — Clicking or tapping a step's pad cycles it through three velocity states in sequence: **Rest → Normal → Accent → Rest**.
- **Visual feedback** — The embedded LED on each pad reflects its current state at rest (dark, muted glow, or bright flare with illuminated border), and briefly flashes white as the playhead strikes it during playback. The central silkscreen text updates dynamically to reflect the current cycle length.

<img width="1500" height="840" alt="velocity_state_cycle" src="https://github.com/user-attachments/assets/a7a73ef8-91d3-4650-a101-216413470f2b" />


### 5.2 Velocity states

The CP-12 uses a discrete velocity system in place of full parameter locking, which dynamically expands when the Duende engine is active:

| State | Appearance | Behaviour |
|---|---|---|
| **0 — Rest** | Dark, flush with the chassis | Step is silent. |
| **0.5 — Ghost** | Node flashes cyan | Triggered algorithmically by the Duende engine. Lower frequency and VCA peak (0.15–0.2 gain) for a subtle background strike. |
| **1 — Normal** | Muted LED glow | Triggers a standard, controlled strike. |
| **2 — Accent** | Bright LED flare, illuminated border | Triggers a hard strike — higher VCA peak and a wider filter cutoff for a sharper, brighter transient. |

> The Ghost state (0.5) cannot be dialled in manually by clicking a pad — it is only ever written by the Duende engine during playback.

---

## 6. The *Duende* (Improv) engine

When active, the Duende engine injects authentic flamenco syncopation into the *compás*. These generative events are visualised dynamically on the sequencer's central dashed guide rings and are re-rolled independently for every step of every pass.

<img width="1240" height="860" alt="duende_probabilities" src="https://github.com/user-attachments/assets/c1e88d88-1077-429f-82f0-aa120ee31165" />

- **Ghost fills** — 15% chance to fill a completely empty step with a subtle foot tap or muted clap. The central guide rings pulse with a watery cyan glow.
- **Syncopated drops** — 10% chance to drop an active beat entirely, creating structural voids. The central guide rings completely dim out.
- **Redobles (double strikes)** — 15% chance to trigger a rapid double-tap (flam), using a fixed 35 ms humanised offset. The central guide rings flash a bright, resonant gold, and the step node itself flashes gold.

Ghost Fills only roll on steps that are silent (Rest) across all three channels; Drops and Redobles only roll on steps where at least one channel is already active — the engine never invents an event on top of nothing, and never doubles up a fill on top of a fill.

---

## 7. Pattern memory reference

The active pattern determines both the rhythmic content of all three channels and the physical geometry of the radial grid.

<img width="2010" height="785" alt="pattern_geometry" src="https://github.com/user-attachments/assets/7faaf0da-aeb1-4c81-be77-876d3b47b1aa" />


| Pattern | Steps | Character |
|---|---|---|
| **Poly 3:2** | 12 | Pure mathematical 3-against-2 polyrhythm — a reference ratio rather than a traditional *palo*. |
| **Bulería 12** | 12 | Standard 12-beat *compás* mapping, the CP-12's default pattern on load. |
| **Bulería 6** | 6 | *Medio compás* variation — a compact 6-beat half-cycle. |
| **Soleá** | 12 | Classic 12-beat ternary *palo* with syncopated accents on the hands. |
| **Alegrías** | 12 | Festive 12-beat *palo* with a denser accent pattern across all three voices. |
| **Seguiriyas** | 12 | Weighted, dramatic 12-beat *palo* with strong accents on beats 1 and 8. |
| **Tangos** | 8 | 8-step binary groove in 4/4 feel. |
| **Fandangos** | 12 | 12-beat *palo* with an even triplet-style foot pulse. |
| **Rumbas** | 8 | 8-step binary groove, a lighter variation of the Tangos feel. |

---

## 8. Technical specifications

- **Sequencer architecture** — Dynamic radial cycle adapting to 6, 8, or 12 steps; mathematically reconstructs geometry (x/y coordinates mapped via percentages for flawless responsive scaling) based on the selected pattern. Includes memory banks for Poly 3:2, Bulería 12, Bulería 6, Soleá, Alegrías, Seguiriyas, Tangos, Fandangos, and Rumbas.
- **Audio engine** — Pure Web Audio API synthesis, driven by a standard lookahead scheduler (25 ms tick, 100 ms schedule-ahead window) referenced against `audioCtx.currentTime` for sample-accurate timing independent of UI thread jitter.
- **Drum synthesis models:**
  - **Foot** — Triangle wave oscillator with a rapid exponential pitch drop into an exponential gain decay; start frequency and peak gain scaled by velocity (including the 0.5 Ghost state).
  - **Right Hand (Seca)** — Band-pass filtered white noise burst, tuned brighter and wider on accent, with slight randomised detuning ("humanise") to avoid mechanically identical hits.
  - **Left Hand (Sorda)** — Low-pass filtered white noise burst, tuned darker and rounder than the right-hand voice.
- **Algorithmic modulation (Duende)** — Introduces a non-destructive 0.5 velocity state for ghost notes and calculates a fixed 35 ms humanised flam offset for redoble grace notes to prevent audio clipping and mechanical stuttering.
- **Interface** — Responsive, hardware-styled radial UI within a rack-mounted chassis. Uses Pointer Event handling for the tempo knob, click/tap handling for step nodes, and dynamic DOM rendering with CSS-driven dashed ring visualisers to seamlessly transition between time signatures.

---

## 9. Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| No sound on first CLOCK / PLAY press | Browsers require a user gesture to start the Web Audio context | Press CLOCK / PLAY again — the first click initialises the audio context and the engine plays back-to-back from the second press onward |
| Pattern sounds "stuck" after switching PATTERN SELECT | Grid rebuild resets `currentStep` to 0 | This is expected behaviour — the new *compás* always restarts from its downbeat |
| Ghost/ghost-note pads won't respond to clicks | By design | Ghost (0.5) is a playback-only state written by the Duende engine; manual clicks only cycle Rest → Normal → Accent |
| Ring flashes but no audible fill | Duende rolled a Drop on a step with only very low-velocity content | Expected — Drops silence the step entirely for that pass only, the underlying pattern is unaffected |

---

## 10. Credits & copyright

**CP-12 *compás* processor**
Created & Developed by José Velázquez MA
Published by Voltage & Wave
Website: [voltageandwave.co.uk](https://voltageandwave.co.uk/)

Copyright © 2026 José Velázquez MA / Voltage & Wave. All rights reserved.
