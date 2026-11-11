# MATLAB PSO tuning files

- `optimization_controllers.m`: top-level PSO tuning and PID/FOPID comparison script.
- `pid_cost.m`: classical PID optimization cost.
- `fopid_cost.m`: fractional-order PID optimization cost.

Run only in an appropriate MATLAB environment with the Control System Toolbox and the matching `fotf` fractional-order toolbox installed.

```matlab
cd('path/to/FOPID-PLC-DC-Motor-Control/matlab')
optimization_controllers
```

**Caution:** The optimizer's active plant uses a second-order/delay model, despite stale first-order comments. The script uses 80 particles and 80 iterations and contains a 15 V actuator limit, while some plot titles mention 24 V. See [documented discrepancies](../docs/REPRODUCIBILITY.md).

The `source_original/` folder retains untouched source text from the Drive folder. The main optimizer's local helper function was moved to the end when creating the `.m` script so its function placement conforms to MATLAB script rules.
