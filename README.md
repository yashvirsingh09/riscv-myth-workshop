# RISC-V MYTH Workshop (Microprocessor for You in Thirty Hours)

[![OS - Linux](https://img.shields.io/badge/OS-Linux-orange)](https://www.linux.org/)
[![Tools - GCC, Spike, Makerchip](https://img.shields.io/badge/Tools-GCC%20%7C%20Spike%20%7C%20Makerchip-navy)](https://makerchip.com)
[![Languages - C, TL-Verilog](https://img.shields.io/badge/Languages-C%20%7C%20TL--Verilog-crimson)](https://tl-x.org)

This repository contains the complete documentation, lab code, testbenches, and hardware models for the **"Microprocessor for You in Thirty Hours" (MYTH)** Workshop, offered by **VLSI System Design (VSD)** and **Redwood EDA**.

---

## 📑 Table of Contents
1. [Course Overview](#-course-overview)
2. [Software to Hardware Transformation Flow](#-software-to-hardware-transformation-flow)
3. [Repository Directory Structure](#-repository-directory-structure)
4. [Day 1: Introduction to RISC-V ISA and GNU Toolchain](#-day-1-introduction-to-risc-v-isa-and-gnu-toolchain)
5. [Day 2: ABI & Basic Verification Flow](#-day-2-abi--basic-verification-flow)
6. [Day 3: Digital Logic with TL-Verilog & Makerchip](#-day-3-digital-logic-with-tl-verilog--makerchip)
7. [Day 4: RISC-V CPU Microarchitecture & Basic Core](#-day-4-risc-v-cpu-microarchitecture--basic-core)
8. [Day 5: Pipelined RISC-V Core & Hazard Resolution](#-day-5-pipelined-risc-v-core--hazard-resolution)
9. [Makerchip Starter Sandboxes & Tools](#-makerchip-starter-sandboxes--tools)
10. [Acknowledgements](#-acknowledgements)

---

## 🔍 Course Overview
The MYTH workshop provides hands-on implementation of a RISC-V microprocessor core from scratch in 5 days (30 hours):
- **Day 1:** Explore the RISC-V ISA, GNU compiler toolchain, C-to-Assembly compilation, and Spike simulation.
- **Day 2:** Understand Application Binary Interface (ABI), system calls, and simulate custom assembly with Picorv32.
- **Day 3:** Master Transaction-Level Verilog (TL-Verilog) and the online Makerchip IDE to build combinational and sequential logic.
- **Day 4:** Design the basic datapath of an RV32I RISC-V CPU (Instruction Fetch, Decode, Register File, ALU).
- **Day 5:** Build a 5-stage pipelined RISC-V core with branch target calculation, data hazards mitigation, and load/store memory operations.

---

## 🔄 Software to Hardware Transformation Flow

```mermaid
flowchart LR
    A["Application Program (C/C++)"] --> B["System Software (Compiler)"]
    B --> C["Assembly Language (RISC-V ISA)"]
    C --> D["Assembler"]
    D --> E["Machine Code (Binary: 0s & 1s)"]
    E --> F["Hardware Core RTL (TL-Verilog)"]
    F --> G["Netlist / Physical Layout (GDSII)"]
```

1. **Application Software:** High-level code written in C/C++.
2. **Compiler (GCC):** Converts high-level language into RISC-V assembly instructions based on the target ISA specifications (`rv64i` / `rv32i`).
3. **Assembler:** Translates instructions into hex/binary machine code.
4. **RTL Implementation (TL-Verilog):** Hardware description implementing the RISC-V microarchitecture specification.
5. **Physical Design (GDSII):** Synthesized netlist placed and routed using open-source EDA flows (e.g. OpenLANE / SkyWater 130nm).

---

## 📂 Repository Directory Structure

```text
riscv-myth-workshop/
├── Day2/                                  # Day 1 & Day 2 C programs, assembly, and testbenches
│   ├── README.md                          # Detailed walkthrough of Day 1 & Day 2 labs
│   ├── Lab1/                              # Basic C compilation & objdump disassembly
│   │   ├── sum1ton.c
│   │   ├── sum1ton_O1.o
│   │   └── sum1ton_Ofast.o
│   ├── Lab2/                              # Number representation (signed vs unsigned arithmetic)
│   │   ├── signhighlow.c
│   │   └── unshighlow.c
│   ├── Lab3/                              # ABI function call testing
│   │   ├── 1to9_custom.c
│   │   └── load.S
│   └── Lab4/                              # Complete Picorv32 simulation testbench
│       ├── 1to9_custom.c
│       ├── load.S
│       ├── picorv32.v
│       ├── riscv.ld
│       ├── rv32im.sh
│       ├── start.S
│       ├── testbench.v
│       └── hex8tohex32.py
│
├── Day3_5/                                # Day 3 to Day 5 TL-Verilog designs & solutions
│   ├── README.md                          # Index and guide for all TL-Verilog designs
│   ├── inverter.tlv                       # CMOS inverter model
│   ├── basicgates.tlv                     # Combinational gates (AND, OR, XOR, NOT)
│   ├── mux.tlv & mux_with_vector.tlv      # 2:1 Multiplexer and vector multiplexers
│   ├── combinational_calculator.tlv       # 4-function combinational calculator
│   ├── pipeline.tlv                       # Pipelining concepts and retiming
│   ├── pythagoras.tlv & pipeline.tlv      # Pipelined distance calculation
│   ├── sequential_calculator.tlv          # Accumulator calculator
│   ├── calculator_singleValueMemory.tlv   # Calculator with memory storage/recall
│   ├── calculator_solutions.tlv           # Complete calculator reference implementation
│   └── risc-v_solutions.tlv               # Complete 5-Stage Pipelined RV32I Processor Core
│
├── calculator_shell.tlv                   # Starter shell for calculator labs
├── risc-v_shell.tlv                       # Starter shell for RISC-V labs
├── reference_solutions.tlv                # Master reference solution file
├── student_projects.md                    # Showcase of alumni MYTH CPU cores
└── tlv_lib/                               # Core TL-Verilog libraries & SVG microarchitecture diagram
```

---

## 🚀 Day 1: Introduction to RISC-V ISA and GNU Toolchain

### Lab 1: Compilation with GCC & Disassembly with Objdump
Compile a standard C program (`sum1ton.c`) using both native GCC and the `riscv64-unknown-elf-gcc` cross-compiler:

```bash
# Compile and run natively
gcc sum1ton.c
./a.out

# Cross-compile for RISC-V target with O1 optimization
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton_O1.o sum1ton.c

# Disassemble to inspect RISC-V assembly instructions
riscv64-unknown-elf-objdump -d sum1ton_O1.o | less

# Simulate using the Spike RISC-V ISA simulator
spike pk sum1ton_O1.o
```

---

## 🛠️ Day 2: ABI & Basic Verification Flow

### Application Binary Interface (ABI)
The ABI establishes the protocol by which application programs interact directly with hardware registers.
- **System Call Calling Conventions:** In RISC-V, integer arguments are passed in registers `a0` - `a7` (`x10` - `x17`), while system call numbers are specified in `a7`.
- **Picorv32 Verification:** In `Day2/Lab4/`, run the `rv32im.sh` script to compile C code, link custom startup assembly (`load.S`, `start.S`), and run the full verilated testbench against the open-source **Picorv32** core.

```bash
cd Day2/Lab4
bash rv32im.sh
```

---

## 💡 Day 3: Digital Logic with TL-Verilog & Makerchip
- **TL-Verilog Benefits:** Eliminates tedious boilerplate, clock and reset wiring, while enabling easy timing retiming with `@` pipeline stages.
- **Makerchip Cloud IDE:** Supports live visualization (VIZ) of digital logic state, waveforms, and automatic block diagrams.
- **Calculator Lab:** Built from a simple 2-input combinational ALU up to a sequential calculator with a 1-cycle memory loop and memory register recall (`Day3_5/calculator_solutions.tlv`).

---

## ⚙️ Day 4 & 5: Complete Pipelined RISC-V Core

Implemented in [`Day3_5/risc-v_solutions.tlv`](Day3_5/risc-v_solutions.tlv):
- **Stage 1 (Fetch):** Program counter generation and instruction memory lookup.
- **Stage 2 (Decode):** Immediate value generation for `R`, `I`, `S`, `B`, `U`, and `J` formats.
- **Stage 3 (Execute):** ALU operations, branch condition evaluation, and branch target calculation.
- **Stage 4 (Memory):** Data Memory (DMem) read/write for `LW` and `SW`.
- **Stage 5 (Write-Back):** Register file write logic with 2-source forwarding logic to prevent RAW read-after-write pipeline hazards.

```mermaid
flowchart LR
    IF["1. Fetch (IF)\nPC & IMem"] --> ID["2. Decode (ID)\nDecoder & RegFile Read"]
    ID --> EX["3. Execute (EX)\nALU & Branch Eval"]
    EX --> MEM["4. Memory (MEM)\nDMem Load / Store"]
    MEM --> WB["5. Write-Back (WB)\nRegFile Write"]
    WB -.->|"Forwarding"| EX
```

---

## 🔗 Makerchip Starter Sandboxes & Tools
- [Makerchip IDE](https://makerchip.com)
- [Calculator Starter Shell](https://myth.makerchip.com/sandbox?code_url=https:%2F%2Fraw.githubusercontent.com%2Fstevehoover%2FRISC-V_MYTH_Workshop%2Fmaster%2Fcalculator_shell.tlv)
- [RISC-V CPU Starter Shell](https://myth.makerchip.com/sandbox?code_url=https:%2F%2Fraw.githubusercontent.com%2Fstevehoover%2FRISC-V_MYTH_Workshop%2Fmaster%2Frisc-v_shell.tlv)
- [Interactive Reference Solutions](https://myth.makerchip.com/sandbox?code_url=https:%2F%2Fraw.githubusercontent.com%2Fstevehoover%2FRISC-V_MYTH_Workshop%2Fmaster%2Freference_solutions.tlv)

---

## 🤝 Acknowledgements
- **Kunal Ghosh**, Co-founder, VLSI System Design (VSD) Corp. Pvt. Ltd.
- **Steve Hoover**, Founder & CEO, Redwood EDA.
- **Shivam Potdar**, CPU Performance Engineer, Workshop Contributor.
