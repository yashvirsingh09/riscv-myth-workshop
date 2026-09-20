# Day 3-5 Labs: Digital Logic with TL-Verilog & RISC-V Core

For students of the **"Microprocessor for You in Thirty Hours" (MYTH)** Workshop, offered by **VLSI System Design (VSD)** and **Redwood EDA**.

---

## 🛠️ Makerchip IDE Instructions
All TL-Verilog code can be simulated and visualized online directly in [Makerchip](https://makerchip.com):
- **Calculator Sandbox Starter:** [Open in Makerchip](https://myth.makerchip.com/sandbox?code_url=https:%2F%2Fraw.githubusercontent.com%2Fstevehoover%2FRISC-V_MYTH_Workshop%2Fmaster%2Fcalculator_shell.tlv)
- **RISC-V Sandbox Starter:** [Open in Makerchip](https://myth.makerchip.com/sandbox?code_url=https:%2F%2Fraw.githubusercontent.com%2Fstevehoover%2FRISC-V_MYTH_Workshop%2Fmaster%2Frisc-v_shell.tlv)

---

## 📁 Included Lab Exercises

### Day 3: Digital Logic with TL-Verilog
| File | Description |
| --- | --- |
| [`inverter.tlv`](inverter.tlv) | Inverter implementation |
| [`basicgates.tlv`](basicgates.tlv) | Basic combinational logic gates |
| [`mux.tlv`](mux.tlv) | 2:1 Multiplexer |
| [`mux_with_vector.tlv`](mux_with_vector.tlv) | Multi-bit vector multiplexer |
| [`combinational_calculator.tlv`](combinational_calculator.tlv) | 4-operation combinational calculator |
| [`counter.tlv`](counter.tlv) | Basic cycle counter |
| [`pipeline.tlv`](pipeline.tlv) | Pipeline timing and retiming |
| [`pythagoras.tlv`](pythagoras.tlv) | Pythagorean theorem calculation |
| [`pythagoras_pipeline.tlv`](pythagoras_pipeline.tlv) | 3-stage pipelined Pythagorean theorem |
| [`total_distance_pythagoras.tlv`](total_distance_pythagoras.tlv) | Distance computation with validity |
| [`cycle_calculator.tlv`](cycle_calculator.tlv) | Calculator with cycle delay |
| [`cycle_calculator_validity.tlv`](cycle_calculator_validity.tlv) | Calculator with validity handling |
| [`sequential_calculator.tlv`](sequential_calculator.tlv) | Sequential calculator with memory loop |
| [`calculator_singleValueMemory.tlv`](calculator_singleValueMemory.tlv) | Calculator with single-value memory recall |
| [`calculator_solutions.tlv`](calculator_solutions.tlv) | **Full Calculator Final Solution** |

---

### Day 4 & Day 5: RISC-V CPU Microarchitecture
| File | Description |
| --- | --- |
| [`risc-v_solutions.tlv`](risc-v_solutions.tlv) | **Complete 5-Stage Pipelined RV32I Processor Core** |

#### Features Implemented in `risc-v_solutions.tlv`:
- **PC & Instruction Fetch (IF):** 32-bit Program Counter with conditional branch redirection.
- **Instruction Decode (ID):** R-type, I-type, S-type, B-type, U-type, and J-type instruction decoding.
- **Register File Read (RF):** 32 x 32-bit registers (x0 hardwired to zero) with 2 read ports and 1 write port.
- **ALU (Arithmetic Logic Unit):** ADD, SUB, SLT, SLTU, XOR, OR, AND, SLL, SRL, SRA.
- **Branch Target Calculation & Resolution:** BEQ, BNE, BLT, BGE, BLTU, BGEU, JAL, JALR.
- **Pipelined Data Forwarding & Hazard Mitigation:** Register bypass forwarding from write-back stage to prevent RAW stalls.
- **Data Memory (DMem) Interface:** Load (LW) and Store (SW) instructions.
