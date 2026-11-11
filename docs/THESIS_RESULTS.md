# Results reported by the original research

**Source:** Kaim Abdeldjalil and Ighil Abderraouf, *Design and Real-Time PLC Implementation of a Fractional-Order PID Controller for DC Motor Speed Control*, master's thesis (2025–2026), University of Boumerdes.

The original thesis PDF is **not hosted in this repository**. The numbers below are attributed reference measurements, **not newly generated results, not tests run by the repository maintainer**, and not verified by rerunning the shared MATLAB script.

## Reported PSO tuning parameters (Chapter 4)

| Gain / parameter | PID (Table 4.2) | FOPID (Table 4.5) |
| --- | ---: | ---: |
| Kp | 0.1795 | 0.3358 |
| Ki | 0.1351 | 0.2542 |
| Kd | 0.0364 | 0.0749 |
| Integral order (lambda) | 1 | 0.9285 |
| Derivative order (mu) | 1 | 0.8855 |
| Optimized cost | 0.9881 | 0.8309 |

## MATLAB nominal step response (Tables 4.3 and 4.6)

| Metric | PID | FOPID |
| --- | ---: | ---: |
| Rise time | 0.3400 s | 0.1400 s |
| Settling time | 0.5800 s | 0.2700 s |
| Overshoot | 0.2911% | 0.1048% |
| Steady-state error | 0.0063 rad/s | 0.0676 rad/s |
| ITAE | 6.2884 | 2.4148 |

## PLC/HIL test at 200 rad/s (Table 5.4)

| Metric | PID Compact | FOPID |
| --- | ---: | ---: |
| Peak speed | 201.51 rad/s | 200.81 rad/s |
| Steady-state speed | 199.97 rad/s | 199.54 rad/s |
| Rise time | 0.4500 s | 0.3150 s |
| Settling time | 0.6600 s | 0.5400 s |
| Overshoot | 0.76% | 0.41% |
| Steady-state error | 0.03 rad/s | 0.46 rad/s |

**Reading the results:** FOPID was faster with lower overshoot in these comparisons, while PID or PID Compact yielded less steady-state error for the reported tables. The MATLAB comparison and PLC/HIL study use different test contexts and must not be treated as the same experiment.

See [reproducibility limitations](REPRODUCIBILITY.md) before comparing a new MATLAB run to these tables.
