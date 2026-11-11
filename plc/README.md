# Siemens TIA Portal / S7-1200 source

[`FB_FOPID_SIL_excerpt.scl`](FB_FOPID_SIL_excerpt.scl) is the **original shared SCL control-loop excerpt** (repackaged under an `.scl` extension for easier reading). It references `DB_Params`, `DB_FOPID` and a simulated first-order motor plant. It is **not** a full TIA Portal project, nor guaranteed to compile without the missing FB interface, data block declarations and proper task configuration.

For the research context and described Siemens hardware, read [PLC implementation notes](../docs/PLC_IMPLEMENTATION.md).

Do not connect actuators based on this excerpt alone.
