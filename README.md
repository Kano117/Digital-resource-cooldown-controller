# Synchronous Resource & Hardware Cooldown Controller

A hierarchical digital controller and arithmetic datapath system designed and simulated in **Intel Quartus Prime**. The design manages real-time resource tracking with upper saturation limits and cycle-accurate cooldown timing.

## Project Overview
This project implements a complete closed-loop digital datapath built from gate-level logic primitives up to complete functional subsystems:
- **5-bit Saturating Resource Accumulator:** Automatically increments by 2 per clock cycle from 0 up to a maximum saturation threshold of 27.
- **Hardware Cooldown Timer:** Tracks elapsed time up to a required 16-cycle duration before permitting subsequent execution.
- **Decision & Validation Logic:** Asserts an active-high ready flag (`F = 1`) only when resource $\ge 12$ and cooldown count $\ge 16$. When a valid trigger request (`use1 = 1`) occurs, the controller deducts 12 resource units and resets the cooldown timer to 0.

---

## Hardware Architecture & Modules
- **D Flip-Flop (DFF):** Gate-level edge-triggered D Flip-Flop with asynchronous `Clear` and `Preset` constructed from basic NAND/NOR logic.
- **Parallel Register (`Res5bi`):** 5-bit synchronous storage register built from chained DFFs.
- **Arithmetic Blocks:**
  - `FullAdder1bit` & 5-bit Ripple Adder (`Fulladder5`)
  - `FullSubtractor1bit` & 5-bit Ripple Subtractor (`Sub5bi`)
- **Control & Routing Logic:**
  - 2:1 Multiplexer (`MUX2_1`) and 5-bit Multiplexer (`Mux5tc`)
  - 5-bit Magnitude Comparator (`SoSanh5bits`)
- **Top-Level Subsystems:**
  - `ManaBlock`: Accumulator managing increment (+2), saturation boundary (27), and conditional deduction (-12).
  - `CoolDown`: Cycle counter tracking the 16-cycle cooldown window.
  - `ManaPlasmaField`: Top-level integration uniting arithmetic datapaths and handshaking logic.

---

## Verification & Waveform Simulation
The circuit was thoroughly simulated using Quartus Waveform Editor, verifying:
1. **Accumulation & Saturation:** Values correctly increment by 2 each cycle and hold steady at 27 without overflow.
2. **Conditional Triggering:** Input requests during cooldown or low-resource states are rejected.
3. **Execution & Reset:** Upon valid trigger, resources drop by 12 and the cooldown timer resets synchronously.

---

## Tools Used
- **EDA Tool:** Intel Quartus Prime (Schematic Capture, Symbol Editor, Waveform Simulator)
- **Target Logic:** Digital CMOS Gate Primitives