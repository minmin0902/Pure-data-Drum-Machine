# Drum Pattern Sequencer in Pure Data

## Abstract
This project is designed to emulate classic drum patterns such as the Amen Break, UK Garage, and the iconic drum pattern from Led Zeppelin's "When The Levee Breaks". Utilizing various Pure Data (PD) techniques, including arrays, counters, and subtractive synthesis, the project explores digital sound generation and manipulation, with an emphasis on rhythmic precision and dynamic sound processing.

## Introduction
The objective of this project was to design and implement a drum sequencer in Pure Data that can efficiently cycle through predefined drum patterns and allow for real-time sound manipulation. The sequencer supports various drum sounds such as kick, snare, and hi-hats and integrates effects like delay and reverb to enhance the audio output.

## Project Description

### System Overview
The sequencer uses an array of 16 steps to store different drum sound combinations encoded as integers (0-6), where each number represents a unique combination of kick (k), snare (s), and hi-hat (h):

- 0: Kick
- 1: Snare
- 2: Hi-hat
- 3: Kick + Snare
- 4: Kick + Hi-hat
- 5: Snare + Hi-hat
- 6: Kick + Snare + Hi-hat

### Sequencer Design
The sequencer operates through a counter subpatch, which increments to cycle through array positions. A modulo operation ensures the counter wraps around at the end of the array. The sequencer's tempo is controlled by a BPM input, and a visual indicator (Hradio object) displays the current step.

### Methodology

#### Sound Generation
Each drum sound is generated using subtractive synthesis:

- **Kick**: Created using an oscillator with a descending pitch, modulated by an amplitude envelope.
- **Snare**: A combination of noise and a pitched oscillator, processed through filters.
- **Hi-hat**: High-pass filtered noise with velocity randomization in the amplitude envelope.

#### Effects Implementation
- **Delay**: Implemented using `delwrite~` and `delread~` objects with adjustable feedback for echo effects.
- **Reverb**: Utilized `rev2~` with controls for level, feedback, and dampening.
- **Filters**: Adjustable cutoff frequency and Q for snare and hi-hat to modify tonal characteristics.

### Pattern Loading and Control
Patterns are loaded into the sequencer using the array set command, with each preset corresponding to a different classic drum pattern. A graphical user interface allows users to select patterns and modify playback in real-time.

## Results
The sequencer successfully emulates the selected drum patterns and allows for dynamic interaction during playback. The addition of effects and real-time control provides a versatile platform for both preset pattern playback and live performance adjustments. This project effectively demonstrates digital signal processing and rhythmic pattern generation, serving as a functional tool for both educational and creative musical applications.

