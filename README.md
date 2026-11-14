# PID vs. Fractional-Order PID for DC Motor Speed Control

**MATLAB PSO optimization · Siemens PLC SCL control-loop excerpt · DC motor speed control**

This project investigates **classical PID** and **fractional-order PID (FOPID)** control of DC motor speed using **MATLAB**, particle swarm optimization (PSO), and a **Siemens PLC Structured Control Language (SCL)** implementation.

**Project:** Ilyes Dinedane · Control engineering / industrial automation

## Project overview

- **Classical PID**: three gains (Kp, Ki, Kd).
- **Fractional PID**: five parameters (Kp, Ki, Kd, lambda, mu).
- **Optimization**: Particle Swarm Optimization (PSO) with an ITAE-based cost, control-effort and saturation penalties.
- **Simulation**: MATLAB controller design and step-response comparison.
- **PLC algorithm**: discrete Grünwald–Letnikov weighted-error sums, actuator saturation, and monitor variables in SCL.

## Files

| Path | Description |
| --- | --- |
| [`matlab/optimization_controllers.m`](matlab/optimization_controllers.m) | PSO tuning and PID/FOPID comparison (local helper moved to end for MATLAB script layout) |
| [`matlab/pid_cost.m`](matlab/pid_cost.m) | PID objective function |
| [`matlab/fopid_cost.m`](matlab/fopid_cost.m) | FOPID objective function |
| [`plc/FB_FOPID_SIL_excerpt.scl`](plc/FB_FOPID_SIL_excerpt.scl) | PLC algorithm excerpt, including simulated motor-model feedback |
| [`source_original/`](source_original/) | Preserved source files from Drive |
| [`docs/PLC_IMPLEMENTATION.md`](docs/PLC_IMPLEMENTATION.md) | Explanation of the supplied PLC excerpt |
| [`docs/REPRODUCIBILITY.md`](docs/REPRODUCIBILITY.md) | Source-level limitations and requirements |
| [`docs/PROVENANCE_AND_RIGHTS.md`](docs/PROVENANCE_AND_RIGHTS.md) | Credits and source origins |

## Control principles

The ideal fractional controller is:

```text
C(s) = Kp + Ki/s^lambda + Kd*s^mu
```

The SCL excerpt computes a finite-window discrete approximation from stored error samples:

```text
u[k] = Kp*e[k] + Ki*sum(Wi[j]*e[k-j]) + Kd*sum(Wd[j]*e[k-j])
```

The code uses error-history indexes 0–20, with weights and parameters provided through external data blocks.

## Running the MATLAB code

Use MATLAB with the Control System Toolbox and a compatible fractional-order `fotf` library. From the `matlab/` directory, run:

```matlab
optimization_controllers
```

**Important:** The optimizer uses a second-order motor model with a Padé-approximated delay and a 15 V actuator bound. Some comments and figure labels in the supplied code describe different settings. The scripts were not executed during repository preparation. Read [source-level reproducibility notes](docs/REPRODUCIBILITY.md) before comparing results.

## Limitations

- This repository does **not** contain a complete native TIA Portal project, all data-block declarations, or a physical I/O configuration.
- The SCL excerpt includes a **simulated plant** update; it is not proof of deployed closed-loop hardware operation.
- No performance claim is independently validated by a new MATLAB or PLC run.
- No open-source license is included; repository availability does not imply unrestricted reuse.

## Project source

The project's MATLAB and PLC source files were supplied by **Ilyes Dinedane** and are organized in this repository for easier review and use. The provided source text is retained under [`source_original/`](source_original/), with readable MATLAB and SCL copies in their respective directories.

This repository contains project code and engineering notes; it does not include a complete native TIA Portal project or independent hardware-test evidence.
