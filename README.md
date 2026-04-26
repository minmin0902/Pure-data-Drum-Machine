# Drum Pattern Sequencer in Pure Data

> A 16-step drum sequencer built in Pure Data with subtractive-synthesized kick, snare, and hi-hat. Includes six classic preset patterns (Amen Break, Led Zeppelin "Levee", UK Garage, etc.), per-instrument velocity sequencing, and live delay/reverb effects.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Made with](https://img.shields.io/badge/made%20with-Pure%20Data-purple.svg)
![Status](https://img.shields.io/badge/status-complete-green.svg)

## Author

**MinJoo Kim**

## Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [System Design](#system-design)
- [Sound Synthesis](#sound-synthesis)
- [Effects](#effects)
- [Presets](#presets)
- [License](#license)

## Overview

A drum machine that cycles through 16 sequencer steps and triggers synthesized drum hits in time. Each instrument (kick, snare, hi-hat) has its own velocity-per-step array, so accents and ghost notes are expressible. Six built-in presets reproduce classic break-beat patterns; live controls let you adjust BPM, effect levels, and instrument volumes during playback.

## Project Structure

```
.
├── DrumMachineGUI.pd   # Main patch — sequencer GUI, preset bank, FX
├── MainTest2.pd        # Working iteration of the main patch
├── MainTest.pd         # Earlier prototype (single combined-step array)
├── counter.pd          # 16-step counter abstraction (BPM-driven)
├── Kick.pd             # Kick synth (descending-pitch oscillator + envelope)
├── snare.pd            # Snare synth (filtered noise + pitched osc)
├── Hat.pd              # Hi-hat synth (high-pass filtered noise)
├── LICENSE
└── README.md
```

## How to Run

1. Install [Pure Data](https://puredata.info/) (vanilla, ≥ 0.51)
2. Open `DrumMachineGUI.pd` in Pd
3. Toggle DSP on
4. Set BPM, choose a preset, and press the start toggle

## System Design

### Step Sequencing

The sequencer uses **three separate 16-step arrays** — one each for kick, snare, and hi-hat:

| Array | Purpose |
|---|---|
| `$0-stepskick` | Kick velocity per step (0 = off) |
| `$0-stepssnare` | Snare velocity per step |
| `$0-stepshat` | Hi-hat velocity per step |

A counter abstraction (`counter.pd`) increments through positions 0–15 driven by a BPM-derived `metro` clock, with modulo wrap-around. The current step is shown live in an Hradio indicator.

> Note: an earlier prototype (`MainTest.pd`) used a single combined `$0-steps` array with integer codes 0–6 representing kick/snare/hat combinations. The final implementation moved to per-instrument velocity arrays for finer expression.

## Sound Synthesis

All drum sounds are generated with **subtractive synthesis** — no samples used.

| Drum | Method |
|---|---|
| **Kick** | Sine oscillator with descending pitch envelope (`osc~` + `vline~`), shaped by amplitude envelope |
| **Snare** | White noise band-passed (`noise~` → `lop~ 3000` → `hip~ 1000`), mixed with a pitched oscillator |
| **Hi-hat** | High-pass filtered noise (`noise~` → `hip~ 20000` → `hip~ 10000`) with velocity-randomized envelope |

## Effects

Built into `DrumMachineGUI.pd`:

| Effect | Implementation | Controls |
|---|---|---|
| Delay | `delwrite~` / `delread~` with feedback | Time, feedback |
| Reverb | `rev2~` | Level, feedback, damping |
| Filters | Adjustable `lop~` / `hip~` on snare and hi-hat | Cutoff, Q |

## Presets

Six classic patterns are loaded by sending the preset name to the array-set system:

| # | Preset | Reference |
|---|---|---|
| 1 | `amenbreak` | The Winstons — "Amen, Brother" |
| 2 | `kickgroove` | Generic four-on-the-floor groove |
| 3 | `house` | Classic house pattern |
| 4 | `ukgarage` | UK Garage shuffle |
| 5 | `rock` | Standard rock backbeat |
| 6 | `ledzep` | Led Zeppelin — "When The Levee Breaks" |

## License

MIT — see [LICENSE](LICENSE) for details.
