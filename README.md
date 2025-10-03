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



## Key Features

- 8-bit Simple CPU Design: Implements the classic SAP-1 architecture with 8-bit data and address buses.
- Manual and Automatic Operation Modes: Supports step-by-step execution for learning and full automatic execution for programs.
- Register Set: Includes A and B registers for ALU operations and temporary data storage.
- Program Counter (PC): Automatically increments to fetch the next instruction from memory.
- Memory System: Contains ROM for program storage and an Address Register for memory access.
- Instruction Register and Opcode Decoder: Separates instruction into opcode and operand, enabling ALU operations.
- Arithmetic Logic Unit (ALU): Performs addition and subtraction operations on the accumulator.
- Control Logic: Includes Timing Control Generator and phase-based operation sequencing.
- Expandable Instruction Set: Supports basic operations like LDA, ADD, SUB, STA, OUT, and HLT.
- Educational Tool: Designed for teaching CPU architecture, instruction cycles, and control logic in a visual environment like Logisim.




