# Reproducibility and source-review notes

The shared MATLAB and SCL code has **not been executed** in MATLAB, TIA Portal, PLCSIM or on a motor during repository preparation.

## Observations directly from supplied source files

1. The active optimization script uses `K_m=38.7`, `tau1=1.07`, `tau2=0.27` and `L=0.03` seconds with a Padé delay model. Some older comments still describe a simpler first-order model or other values.
2. The code sets `V_max=15`, while some comments and plot titles refer to 24 V. Use the actual active variable in calculations.
3. The script uses **80 particles and 80 iterations** for the PID and FOPID PSO searches. These are source settings, not independently validated optimum values.
4. The PID objective simulates **0–8 s**, while the FOPID objective simulates **0–5 s**. The final comparison uses 0–5 s; the optimization horizons therefore differ.
5. The shared `optimization controllers.txt` includes a local `get_metrics` function before later script statements. The GitHub `.m` copy moves it to the end of the file to follow MATLAB script-function placement rules. Its equations are preserved; consult [original source](../source_original/) for comparison.
6. The PLC excerpt computes motor speed from a **discrete motor model** rather than a physical sensor signal. The excerpt references external data blocks but does not provide full declarations, I/O mapping or a native project.
7. The MATLAB code requires a compatible `fotf` fractional-order toolbox.

## How to validate a future run

- Document MATLAB, Control System Toolbox and fractional-order toolbox versions.
- Set consistent plant and optimization horizons if performing a controlled comparison; record any changes.
- Save parameters, random seed, convergence history, response arrays and comparison plots from the actual run.
- Compile and simulate PLC code only after obtaining or rebuilding the missing data blocks and task configuration.
- Do not energize real hardware before checking physical I/O, safety conditions and motor-drive limits.

This repository contains the original shared sources and a clearly labeled convenience adaptation, not newly reproduced performance data.
