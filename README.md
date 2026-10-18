# CMOS Inverter Simulation in LTspice

A basic CMOS inverter was assembled and simulated in LTspice using complementary NMOS and PMOS transistor models. The project includes two configurations: **without an output load** and **with a 10 kΩ output load**. DC operating-point and transient analyses were also explored.

> **Project status:** Educational simulation and observation. The recorded transient output did not reach a clean logic-low level when the input was high, so the results are documented as observed rather than presented as a fully validated inverter.

## Objectives

- Construct a CMOS inverter using NMOS and PMOS transistors.
- Apply a 0–5 V pulse input and observe input/output waveforms.
- Compare operation with and without a resistive output load.
- Inspect the DC operating point and explore AC analysis.
- Record simulation limitations and identify checks needed for further validation.

## Circuit design

| Parameter | Configuration |
|---|---|
| Supply voltage | 5 V |
| NMOS | M1, model `NM`, L = 10 µm, W = 10 µm |
| PMOS | M2, model `PM`, L = 10 µm, W = 20 µm |
| Input | Common gate connection of M1 and M2 |
| Output | Common drain connection of M1 and M2 |
| PMOS source | Connected to +5 V |
| NMOS source | Connected to ground |
| Input pulse | `PULSE(0 5 2m 1n 1n 5m 10m)` |
| Transient command | `.tran 20m` |
| Loaded configuration | 10 kΩ resistor from output node to ground |

### Device models

```spice
.model NM NMOS (LEVEL=1 VTO=1 KP=200u LAMBDA=0.02)
.model PM PMOS (LEVEL=1 VTO=-1 KP=200u LAMBDA=0.02)
```

These are simple Level 1 educational models, not foundry-validated models for a specific semiconductor process.

## Simulation configurations

### 1. CMOS inverter without load

The initial circuit contains the PMOS pull-up transistor and NMOS pull-down transistor, with no additional output load resistor.

**Observed transient behavior:**
- The input switches between approximately 0 V and 5 V.
- The output remains high for much of the displayed simulation.
- The output waveform moves to roughly 4.3 V during part of the input-high interval instead of falling close to 0 V.

This does not show the expected full logic swing of a CMOS inverter. It is retained as an observation from the current simulation, not as proof of correct inverter switching.

### 2. CMOS inverter with 10 kΩ load

A **10 kΩ resistor (R1)** was added between the output node and ground. This provides a resistive load on the output and allows a qualitative comparison with the unloaded circuit.

**Observed transient behavior:**
- The input continues to switch between approximately 0 V and 5 V.
- The plotted output remains mostly high, with levels around 4.3–4.7 V in the displayed intervals.
- The output still does not reach a clean logic-low level when the input is high.

The loaded waveform therefore does not demonstrate correct full-swing inverter operation either. The load changes the output network, but the current results are not sufficient to make a reliable quantitative claim about load-induced delay, power, or drive strength.

### Loaded versus unloaded: summary

| Feature | Without load | With 10 kΩ load |
|---|---|---|
| Output load | No added resistor | 10 kΩ from output to ground |
| Input swing | Approximately 0–5 V | Approximately 0–5 V |
| Observed output | Mostly high; around 4.3–5 V in the plot | Mostly high; around 4.3–4.7 V in the plot |
| Full logic swing observed? | No | No |
| Interpretation | Switching behavior requires further verification | Loaded behavior also requires further verification |

**Expected ideal logic behavior**

| Input | Expected output |
|---|---|
| 0 V (logic 0) | Near 5 V (logic 1) |
| 5 V (logic 1) | Near 0 V (logic 0) |

## DC operating-point analysis

With the input at 0 V, the LTspice operating-point report showed:

| Measurement | Observed value |
|---|---:|
| V(n001) | 5 V |
| V(n002) | 0 V |
| V(n003) | 5 V |
| I(V1) | −1.001 × 10⁻¹¹ A |
| I(V2) | 0 A |
| Id(M1) | 1.001 × 10⁻¹¹ A |
| Id(M2) | 9.81503 × 10⁻¹² A |

The output node is at 5 V for the low input, which agrees with the expected logic state. The extremely small transistor currents should be treated cautiously and checked against the schematic connections and model setup.

## AC analysis attempt

An exploratory AC analysis was attempted with a 2.5 V DC input bias, AC amplitude of 1, and:

```spice
.ac dec 100 1 100Meg
```

The reported output DC level was approximately 4.52 V. This run was not established as a valid small-signal measurement around a verified switching-region bias point. Therefore, this repository does not claim a validated voltage gain, bandwidth, or cutoff frequency from that run.

## Reproducing the simulations

1. Open the LTspice schematic.
2. Confirm the common gate input, common drain output, PMOS source at +5 V, and NMOS source at ground.
3. Confirm the model names are `NM` and `PM`.
4. For the unloaded case, run the circuit without R1.
5. For the loaded case, add a 10 kΩ resistor from the output node to ground.
6. Use the pulse source `PULSE(0 5 2m 1n 1n 5m 10m)` and run `.tran 20m`.
7. Compare the input and output waveforms and verify that the output reaches both logic levels before describing the inverter as functional.

## Repository structure

- `README.md` — circuit setup, loaded/unloaded comparison, analyses, observations, and limitations.
- `models/inverter_models.lib` — NMOS and PMOS model definitions.
- `schematic/` — location for the original LTspice `.asc` files.
- `results/` — location for screenshots of both circuit configurations, the transient waveforms, and the operating-point report.

The screenshots shared for this update were used to document the configurations and visible results above. The original editable LTspice schematic files have not been added by this documentation update.

## Tools and concepts

- LTspice
- CMOS inverter topology
- NMOS and PMOS Level 1 models
- DC operating-point analysis
- Transient analysis
- Exploratory AC analysis
- Resistive output loading

## Conclusion

The CMOS inverter was explored in LTspice in both unloaded and 10 kΩ-loaded configurations. The input waveform switches as intended, but the recorded output remains mostly high rather than reaching the expected low logic level. This is an important limitation of the current simulation and should be resolved before using the results to claim correct inverter operation or to report meaningful performance measurements.
