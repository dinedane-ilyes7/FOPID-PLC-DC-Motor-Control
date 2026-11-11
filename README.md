# Fractional-Order PID (FOPID) for DC Motor Speed Control

**MATLAB/PSO optimization · Siemens S7-1200 PLC · Grünwald–Letnikov discretization**

This repository is a **credited technical study and source archive** of an industrial motor-speed control project comparing conventional **PID** with **fractional-order PID (PI^λD^μ)**. It brings together the code shared in a [Google Drive project folder](https://drive.google.com/drive/folders/11zTw4uKzmxQ4Z31S2awmMDKce1stDv-_) and technical explanations derived from the accompanying master's thesis.

**Original research authors:** **Kaim Abdeldjalil and Ighil Abderraouf**. **Supervisor:** Dr. Haddouche Rezki. **Institution:** IGEE / University of Boumerdes, Algeria (2025–2026).

> **Provenance:** This repository is not presented as research personally performed by its GitHub maintainer. The thesis PDF was used **only as a technical reference and has not been uploaded**. MATLAB and PLC source files are credited to the original shared project. No open-source license is asserted for that third-party work.

## Overview

- **Plant:** DC motor speed regulation
- **Controllers:** PID (3 gains) versus FOPID (5 parameters: `Kp`, `Ki`, `Kd`, `lambda`, `mu`)
- **Tuning:** Particle Swarm Optimization (PSO) in MATLAB with the fractional transfer-function (`fotf`) toolbox
- **Discretization:** Truncated Grünwald–Letnikov weighted error history
- **Industrial platform described in the thesis:** Siemens S7-1200 CPU 1215C DC/DC/DC, TIA Portal, periodic **10 ms** task
- **Evaluation:** Reference tracking, overshoot, settling time, steady-state error, disturbance response

## Repository contents

| Folder / file | Description |
| --- | --- |
| [`matlab/optimization_controllers.m`](matlab/optimization_controllers.m) | MATLAB PSO optimization and PID–FOPID comparison; one local function relocated to the script end for MATLAB syntax |
| [`matlab/pid_cost.m`](matlab/pid_cost.m) | PID PSO cost function |
| [`matlab/fopid_cost.m`](matlab/fopid_cost.m) | FOPID PSO cost function |
| [`plc/FB_FOPID_SIL_excerpt.scl`](plc/FB_FOPID_SIL_excerpt.scl) | Siemens SCL excerpt with fractional weighted sums and motor-model simulation; **not** a complete TIA Portal project |
| [`source_original/`](source_original/) | Source-text originals supplied through Drive |
| [`docs/PLC_IMPLEMENTATION.md`](docs/PLC_IMPLEMENTATION.md) | PLC architecture and hardware context described by the thesis |
| [`docs/THESIS_RESULTS.md`](docs/THESIS_RESULTS.md) | Reported PID versus FOPID metrics, **not independently remeasured** |
| [`docs/REPRODUCIBILITY.md`](docs/REPRODUCIBILITY.md) | Known source/script differences, missing project files, and validation limits |
| [`docs/PROVENANCE_AND_RIGHTS.md`](docs/PROVENANCE_AND_RIGHTS.md) | Source and attribution details |

## Controller model

The fractional PID law in the research is:

```text
C(s) = Kp + Ki / s^lambda + Kd * s^mu
```

The PLC excerpt uses finite Grünwald–Letnikov sums on a rolling buffer of recent tracking errors:

```text
u[k] = Kp * e[k] + Ki * sum(Wi[j] * e[k-j]) + Kd * sum(Wd[j] * e[k-j])
```

The shared SCL excerpt uses buffer positions **0..20**; this is 21 current/past values. The thesis describes a truncation length of 20 samples, so its indexing conventions must be reconciled before commissioning.

## Results reported in the thesis

| MATLAB step metric | PID | FOPID |
| --- | ---: | ---: |
| Rise time | 0.340 s | 0.140 s |
| Settling time | 0.580 s | 0.270 s |
| Overshoot | 0.2911% | 0.1048% |
| Steady-state error | 0.0063 rad/s | 0.0676 rad/s |

These are **reported research results**, not a verification run from this repository. FOPID had faster nominal transients in the cited study; PID had lower steady-state error in the reported table.

## Re-running the MATLAB examples

1. Install MATLAB, Control System Toolbox, and the compatible **FOTF/FOMCON** fractional-order transfer-function toolbox.
2. Place the `matlab/` folder on the MATLAB path.
3. Run `optimization_controllers.m`.
4. Read [reproducibility notes](docs/REPRODUCIBILITY.md) before interpreting the resulting gains or plots. **These scripts have not been executed or independently validated during this publication workflow.**

The shared optimization source has discrepancies in model captions, voltage labels, PSO settings and simulation intervals. Preserving and documenting these differences is preferable to silently modifying scientific results.

## What is *not* uploaded

- **No thesis PDF** (reference only, as requested)
- No original report cover or author photos
- No full native TIA Portal project or complete PLC data blocks
- No claim of new experimental hardware validation
- The original Drive figures remain available from the [source folder](https://drive.google.com/drive/folders/11zTw4uKzmxQ4Z31S2awmMDKce1stDv-_); the repo does not republish the images

## Attribution and rights

Research: **Kaim Abdeldjalil and Ighil Abderraouf** (2025–2026), supervisor **Dr. Haddouche Rezki**. These materials are shared for learning and technical discussion with source attribution. **The GitHub maintainer should confirm authorization to publicly redistribute the original code.** No license is assigned to the original authors' materials here.

See [provenance](docs/PROVENANCE_AND_RIGHTS.md) and [known limitations](docs/REPRODUCIBILITY.md).
