# SAP-1-Architecture-Logisim
## Table of Contents
Click on the Table of Contents below to directly go to the sections:

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Key Features](#key-features)

- [Architecture and Functional Block Analysis](#architecture-and-functional-block-analysis)
  - [System Architecture Overview](#system-architecture-overview)
  - [Register Implementation (A, B)](#register-implementation-a-b)
  - [Program Counter (PC) Implementation](#program-counter-pc-implementation)
  - [Memory System and Address Register](#memory-system-and-address-register)
  - [Instruction Register and Opcode Decoder](#instruction-register-and-opcode-decoder)
  - [Arithmetic Logic Unit (ALU) Implementation](#arithmetic-logic-unit-alu-implementation)
  - [Boot/Loader Counter and Phase Generation](#bootloader-counter-and-phase-generation)

- [Control System Design](#control-system-design)
  - [Timing Control Generator](#timing-control-generator)
  - [Automatic Operation Control Logic](#automatic-operation-control-logic)
  - [Manual/Loader Operation Control](#manualloader-operation-control)

- [Instruction Set Architecture](#instruction-set-architecture)
  - [Instruction Encoding Scheme](#instruction-encoding-scheme)
  - [Assembler](#assembler)

- [Operation](#operation)
  - [Fetch–Decode–Execute Cycle](#fetchdecodeexecute-cycle)
  - [Running the CPU in Manual Mode](#running-the-cpu-in-manual-mode)
  - [Running the CPU in Automatic Mode (JMP + ADD Program)](#running-the-cpu-in-automatic-mode-jmp--add-program)

- [Future Improvement](#future-improvement)
- [Conclusion](#conclusion)

- 

## Key Features

- **8-bit CPU Architecture:** Implements the classic SAP-1 design with 8-bit data and address buses.  
- **Manual & Automatic Modes:** Allows step-by-step instruction execution and fully automatic program run.  
- **Register Set:** Includes **A and B registers** for ALU operations and temporary storage.  
- **Program Counter (PC):** Automatically increments to fetch the next instruction from memory.  
- **Memory System:** Combines **ROM for program storage** and an **Address Register** for accessing memory.  
- **Instruction Register & Opcode Decoder:** Separates instructions into opcode and operand for proper ALU operations.  
- **Arithmetic Logic Unit (ALU):** Performs **addition and subtraction** on the accumulator.  
- **Control Logic:** Features **Timing Control Generator** and phase-based operation sequencing.  
- **Expandable Instruction Set:** Supports basic instructions like **LDA, ADD, SUB, STA, OUT, HLT**.  
- **Educational Tool:** Perfect for learning CPU architecture, instruction cycles, and control logic using **Logisim visualization**.



## Architecture and Functional Block Analysis

### System Architecture Overview
The processor architecture uses a **unified single-bus design** with an **8-bit datapath** controlled by tri-state sources. Bus arbitration ensures that only **one driver** is active during each T-state, with possible drivers including `pc_out`, `sram_rd`, `ins_reg_out_en`, `a_out`, `b_out`, `alu_out`, and `sh_out`. Bus listener components such as `mar_in_en`, `ins_reg_in_en`, `a_in`, `b_in`, and `sram_wr` allow selective data capture when required.

**Figure 1:** Automatic mode operation of the control sequencer showing fetch–decode–execute sequencing.  
![Automatic Mode Control Sequencer](images/automatic_ckt.png)

**Figure 2:** Manual/Loader mode operation of the control sequencer showing secure program loading with debug and handshake signals.  
![Manual/Loader Mode Control Sequencer](images/manual_ckt.png)

### Register Implementation (A, B)

The **A and B registers** use `reg_gp` modules to store **8-bit data**. They have three interfaces:

1. **Input:** Connected to the system bus; controlled by `a_in` and `b_in` signals.  
2. **Output:** Drives the bus via tri-state logic using `a_out` and `b_out`.  
3. **Internal:** Provides direct access to the **ALU** through `reg_int_out` without using the bus.

**Figure 3:** A/B register subsystem with input, output, and internal interfaces.  
![A/B Register Subsystem](images/2_ins_reg_.png)

### Program Counter (PC) Implementation

The **Program Counter (PC)** supports **dual modes** for sequential execution and program flow control:

- **Increment Mode:** At timing state `T3`, when `pc_en = 1`, the PC updates as `PC ← PC + 1`, enabling sequential instruction progression.  
- **Jump Mode:** During a `JMP` instruction at timing state `T4`, when `jump_en = 1`, the lower nibble of the Instruction Register (IR) is placed on the bus and loaded into the PC for direct program control transfer.  

**Bus Interface:** At timing state `T1`, when `pc_out = 1`, the current PC value is driven onto the system bus and loaded into the **Memory Address Register (MAR)** to start the instruction fetch cycle.

**Figure 4:** Program Counter showing **increment and jump modes**.  
![Program Counter Increment/Jump](images/pc_a.png)

**Figure 5:** Program Counter direct load and sequential functionality.  
![Program Counter Direct Load](images/pc_b.png)

### Memory System and Address Register

The memory subsystem includes a **4-bit Memory Address Register (MAR)** which captures addresses from the system bus under the control of `mar_in_en`.

- **Instruction Fetching:** During the `T1` phase, `pc_out` and `mar_in_en` load the Program Counter value into the MAR for instruction fetch.  
- **Operand Addressing:** During `T4` of `LDA`, `LDB`, `STA`, or `JMP` instructions, `ins_reg_out_en` together with `mar_in_en` transfers the operand address (`IR[3:0]`) into the MAR.

The **SRAM** operates in two modes:  
- **Read Mode:** When `sram_rd = 1`, the value at `RAM[MAR]` is placed on the bus during `T2` (instruction fetch) and `T5` (LDA/LDB).  
- **Write Mode:** When `sram_wr = 1`, the data on the bus is written into `RAM[MAR]` during `T5` of the `STA` instruction.

**Figure 6:** Register-based memory element showing `data_in`, `wr_en`, `rd_en`, clock, and chip select (`cs`) signals. Output to the bus is via `data_out`.  
![Memory Element](images/fig_5.png)

**Figure 7:** Memory subsystem showing MAR operation and SRAM read/write timing.  
![Memory Subsystem](images/fig_6.png)






