# CMOS Inverter Simulation Using LTspice

## Overview

This project explores the design and simulation of a CMOS inverter using complementary NMOS and PMOS transistors in LTspice. The circuit was simulated in two configurations—**without an external load** and **with a 10 kΩ output load**—to observe the input/output waveforms and study the effect of output loading.

## Objectives

- Build a CMOS inverter using NMOS and PMOS transistor models.
- Apply a pulse input and observe the transient response.
- Simulate and compare the circuit with and without an output load.
- Inspect the DC operating point.
- Gain practical experience with LTspice schematic design and simulation analysis.

## Circuit specifications

| Parameter | Value |
|---|---|
| Supply voltage (VDD) | 5 V |
| NMOS transistor | M1 |
| NMOS model | NM |
| NMOS dimensions | L = 10 µm, W = 10 µm |
| PMOS transistor | M2 |
| PMOS model | PM |
| PMOS dimensions | L = 10 µm, W = 20 µm |
| Input source | `PULSE(0 5 2m 1n 1n 5m 10m)` |
| Transient analysis | `.tran 20m` |
| Loaded configuration | 10 kΩ resistor from output to ground |

## MOSFET model definitions

The simulation uses basic Level 1 MOSFET models:

```spice
.model NM NMOS (LEVEL=1 VTO=1 KP=200u LAMBDA=0.02)
.model PM PMOS (LEVEL=1 VTO=-1 KP=200u LAMBDA=0.02)
```

These simplified models are intended for introductory circuit simulation and learning.

## Circuit configurations

### 1. CMOS inverter without load

The initial circuit was simulated with the NMOS and PMOS transistors connected in a complementary inverter arrangement. A pulse source was applied to the common gate input, and the output waveform was observed using transient analysis.

**What was explored**
- CMOS inverter topology and transistor connections
- Pulse input switching between 0 V and 5 V
- Input and output waveforms over a 20 ms simulation interval

### 2. CMOS inverter with a 10 kΩ load

A 10 kΩ resistor was connected from the output node to ground, and the circuit was simulated again.

**What was explored**
- Output behavior with a resistive load
- Comparison of the loaded and unloaded circuit waveforms
- How an external load changes the output network

## Loaded and unloaded comparison

| Feature | Without load | With 10 kΩ load |
|---|---|---|
| Supply voltage | 5 V | 5 V |
| Input signal | 0–5 V pulse | 0–5 V pulse |
| Output load | No added resistor | 10 kΩ to ground |
| Analysis | Transient | Transient |
| Main focus | Observe inverter switching response | Observe response with an output load |

The simulations provide a practical comparison of the same CMOS inverter topology under two output-loading conditions.

## DC operating-point analysis

The LTspice operating-point analysis was also run to inspect the circuit's node voltages and transistor currents. With the input at 0 V, the reported node voltages included:

| Measurement | Simulated value |
|---|---:|
| V(n001) | 5 V |
| V(n002) | 0 V |
| V(n003) | 5 V |
| Drain current of M1 | Approximately 1.001 × 10⁻¹¹ A |
| Drain current of M2 | Approximately 9.815 × 10⁻¹² A |

These values are recorded from the LTspice operating-point report.

## AC analysis

An AC analysis was also explored using a 2.5 V DC input bias, an AC amplitude of 1, and the following directive:

```spice
.ac dec 100 1 100Meg
```

This was part of the practical exploration of LTspice's available analysis modes.

## Simulation workflow

1. Draw the CMOS inverter schematic in LTspice.
2. Connect the PMOS to the positive supply and the NMOS to ground.
3. Connect the transistor gates to the input and the drains to the output.
4. Define the NMOS and PMOS models.
5. Apply the pulse input and run transient analysis.
6. Observe the input and output waveforms.
7. Add a 10 kΩ resistor at the output and run the loaded configuration.
8. Run the DC operating-point analysis to inspect node voltages and transistor currents.

## Tools and concepts

- **Software:** LTspice
- **Circuit:** CMOS inverter
- **Devices:** NMOS and PMOS MOSFETs
- **Analyses:** Transient, DC operating point, and exploratory AC analysis
- **Concepts practiced:** CMOS switching, transistor-level schematic design, pulse sources, output loading, and waveform interpretation

## Conclusion

A CMOS inverter was designed and simulated in LTspice using NMOS and PMOS transistor models. Both unloaded and 10 kΩ-loaded configurations were explored, along with transient waveform observation and DC operating-point analysis. This exercise provided practical experience with transistor-level circuit construction, simulation setup, and interpreting circuit responses under different loading conditions.
