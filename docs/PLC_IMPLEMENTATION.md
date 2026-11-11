# Siemens S7-1200 fractional-order controller — technical summary

**Original study authors:** Kaim Abdeldjalil and Ighil Abderraouf (University of Boumerdes, 2025–2026). The thesis is a reference only and is not published here.

## Described experimental platform

| Component | Thesis description |
| --- | --- |
| PLC | Siemens SIMATIC S7-1200 CPU 1215C DC/DC/DC (6ES7 215-1AG40-0XB0) |
| Motor trainer | Feedback Instruments MS150 platform / MT150F DC motor |
| Servo amplifier | SA150D |
| Speed sensing | GT150X tachogenerator |
| Software | Siemens TIA Portal, SCL, PLCSIM |
| Scheduled control task | OB30 cyclic interrupt, 10 ms |

## Fractional-order control

The discrete Grünwald–Letnikov (GL) implementation uses finite weighted sums of past errors:

```text
e[k] = reference[k] - speed[k]

u[k] = Kp*e[k]
     + Ki*sum(Wi[j]*e[k-j])
     + Kd*sum(Wd[j]*e[k-j])
```

In [the provided SCL excerpt](../plc/FB_FOPID_SIL_excerpt.scl), the error-history array is indexed `0..20` (21 stored values: current + 20 past errors). The reference thesis uses the phrase “20-sample memory”, which needs an exact indexing convention when reconstructing the block interface.

The excerpt references `DB_Params`, `DB_FOPID`, precomputed `W_i` and `W_d` values, plus first-order motor-model coefficients. It computes saturation and monitor signals. The excerpt's `omega` update is a **simulated first-order plant**, so it does not alone prove live tachogenerator feedback or physical closed-loop operation.

## TIA Portal architecture described in the thesis

- **OB30**: cyclic scheduling of reference/feedback, control function, and monitoring.
- **FB_FOPID**: fractional weighted sums, control output and bounds.
- **Motor-model simulation function**: discrete first-order plant used in SIL testing.
- **Data blocks**: parameters, precomputed weights, history arrays, monitor variables.

## Analog scaling reported in the thesis

The report describes `IW64` as an analog feedback input spanning 0–27648 digital counts for 0–10 V, with speed conversion up to the test system's nominal 530 rad/s. It discusses output `QW66` and a 0–20 mA interface for the motor drive.

**Do not commission real hardware from this summary.** Verify actual installed I/O modules, electrical connections, analog scaling, motor drive limits, emergency stop and watchdog behavior against device documentation.

## Missing native project files

No native TIA Portal project archive, full FB/DB declarations, hardware configuration, complete wiring drawing or tested real-hardware deployment code was present in the linked Drive folder. The [SCL source](../plc/FB_FOPID_SIL_excerpt.scl) is consequently labeled **excerpt**, not a complete runnable PLC program.
