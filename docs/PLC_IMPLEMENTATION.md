# PLC SCL control-loop excerpt

This page explains the supplied [SCL source](../plc/FB_FOPID_SIL_excerpt.scl), not a verified complete TIA Portal project.

## Algorithm in the source file

1. Compute tracking error `e_now = DB_Params.SP - DB_FOPID.omega`.
2. Shift stored error values and insert the newest error at index 0.
3. Compute fractional integral and derivative sums using `DB_Params.W_i` and `DB_Params.W_d`.
4. Combine the proportional, fractional integral and fractional derivative terms with `Kp`, `Ki` and `Kd`.
5. Saturate the commanded output to configured `U_min` / `U_max`.
6. Update `omega` using the provided **first-order simulated motor-model** coefficients `Motor_a` and `Motor_b`.
7. Copy telemetry into monitor variables in `DB_Params`.

## Discrete fractional approximation

The error-history buffer has positions **0..20** (21 values including the current error). The code loops over all these positions for both weighted sums. Weight generation and all parameter/data-block declarations are not present in the shared excerpt.

## Running on hardware

The code uses Siemens-style Structured Control Language (SCL) and data-block names but **is not independently compilable or deployable** without the missing TIA Portal project elements. Real-hardware operation would additionally require verified tachometer input scaling, motor-drive output scaling, protection, safe-stop behavior and the correct PLC configuration. None of those physical integration details is verified by this source excerpt.

See [reproducibility notes](REPRODUCIBILITY.md) and the [original source archive](../source_original/).
