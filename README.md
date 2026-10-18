# CMOS Inverter Simulation in LTspice

A CMOS inverter study built in LTspice using one NMOS transistor and one PMOS transistor. This repository records the circuit setup, device models, analyses attempted, and the results observed.

> **Status:** Learning / exploratory simulation. The transient output did not reach a clean logic-low level in the recorded run. This is documented honestly rather than presented as a fully validated inverter.

## Objective

- Build a CMOS inverter using complementary NMOS and PMOS transistors.
- Apply a pulse input and observe the output.
- Inspect the DC operating point and explore transient and AC analyses.
- Compare observed behavior with expected inverter operation.

## Circuit configuration

| Item | Value / connection |
|---|---|
| Supply voltage (VDD) | 5 V |
| NMOS | M1, model `NM`, L = 10 µm, W = 10 µm |
| PMOS | M2, model `PM`, L = 10 µm, W = 20 µm |
| Input | Shared gates of M1 and M2 |
| Output | Shared drains of M1 and M2 |
| PMOS source | +5 V |
| NMOS source | Ground |
| Transient input | `PULSE(0 5 2m 1n 1n 5m 10m)` |
| Transient command | `.tran 20m` |

## MOSFET models

The circuit used these simple Level 1 models:

```spice
.model NM NMOS (LEVEL=1 VTO=1 KP=200u LAMBDA=0.02)
.model PM PMOS (LEVEL=1 VTO=-1 KP=200u LAMBDA=0.02)
```

These are educational models, not foundry-validated models for a particular fabrication process. A copy is included in [models/inverter_models.lib](models/inverter_models.lib).

## Analyses and observations

### 1. DC operating point (`.op`)

With the input at 0 V, LTspice reported approximately:

| Measurement | Observed value |
|---|---:|
| Input DC voltage | 0 V |
| Output DC voltage | 5 V |
| NMOS drain current | (1.001 \times 10^{-11}) A |
| PMOS drain current | (9.815 \times 10^{-12}) A |

A high output for a low input agrees with the expected inverter logic state. The very small reported currents are worth checking against the exact schematic wiring, model assignment, and operating-point setup.

### 2. Transient analysis

The input pulse and transient command were:

```spice
PULSE(0 5 2m 1n 1n 5m 10m)
.tran 20m
```

**Observed:** the input switched between 0 V and 5 V, while the output stayed mostly high, approximately 4.3–5 V, rather than reaching a clean low level when the input was high.

**Interpretation:** this is not the expected full logic swing of a correctly operating CMOS inverter. The original wiring, transistor pin order, model assignments, and selected output node need to be checked before claiming correct switching.

### 3. AC analysis attempt

An exploratory AC analysis used a DC input bias of 2.5 V, AC amplitude 1, and:

```spice
.ac dec 100 1 100Meg
```

The output DC level was approximately 4.52 V. This run was not established as a valid small-signal characterization around a verified switching-region bias point, so no gain, bandwidth, or cutoff-frequency claim is made from it.

## Expected logic behavior

| Input | Expected output |
|---|---|
| 0 V (logic 0) | Near VDD (logic 1) |
| VDD (logic 1) | Near 0 V (logic 0) |

A working inverter should switch its output in the opposite direction to its input. The transient result recorded here did not fully meet that expectation.

## Repository structure

- `README.md` — project setup, analyses, observations, and limitations.
- `models/inverter_models.lib` — Level 1 NMOS and PMOS model statements.
- `schematic/` — add the original LTspice `.asc` file here.
- `results/` — add screenshots of the schematic, operating point, transient input/output waveforms, and AC plot here.

The original schematic and screenshots need to be uploaded from the actual LTspice files; they were not available to this GitHub update, so they have not been fabricated or recreated.

## Reproducing the simulation

1. Open the original CMOS inverter schematic in LTspice.
2. Check that the gates share the input, the drains share the output, the PMOS source is connected to +5 V, and the NMOS source is grounded.
3. Ensure the device model names match `NM` and `PM`.
4. Run `.op` to inspect the DC operating point.
5. Run `.tran 20m` with the pulse source to inspect switching.
6. Save screenshots of the schematic and plots into `results/`.
7. Confirm that the output reaches both logic levels before describing the inverter as fully functional.

## Tools

- LTspice
- Level 1 MOSFET models
- DC operating-point, transient, and AC analysis

## Project status

The CMOS inverter topology was assembled and several analyses were attempted. The transient output did not show the expected full logic swing, and the AC run was exploratory rather than a validated gain/bandwidth measurement.
