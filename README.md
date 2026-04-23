# RISC-V Single-Cycle Core Processor

This repository contains a small educational RISC-V single-cycle processor described in Verilog. Each hardware block is documented in its own Markdown file, with the Verilog source embedded inside a fenced code block.

The design models the main datapath components of a basic single-cycle CPU:

- program counter
- instruction memory
- control unit
- register file
- ALU
- data memory
- PC adder

## Project Goal

The purpose of this project is to show how a simple RISC-V processor can be built from separate modules that work together in one clock cycle. It is best suited for learning and small-scale simulation rather than as a complete production-ready core.

## Architecture Overview

In a single-cycle processor, one instruction is fetched, decoded, executed, and written back in a single clock cycle.

High-level data flow:

1. The program counter (`PC`) holds the address of the current instruction.
2. Instruction memory returns the instruction stored at that address.
3. The control unit decodes the opcode and generates datapath control signals.
4. The register file reads the source operands.
5. The ALU performs arithmetic, logic, comparison, or address calculation.
6. Data memory is accessed for load/store instructions.
7. The result is written back to the register file.
8. The next PC is selected for sequential execution or branching.


## Files

Each file below contains documentation plus the Verilog for one module:

| File | Module | Purpose |
| --- | --- | --- |
| `PC.md` | `P_C` | Stores the current program counter value |
| `PC_ADDER.md` | `PC_Adder` | Adds two 32-bit values such as `PC + 4` |
| `Instruction Memory.md` | `Instr_Mem` | Returns the instruction at the current PC |
| `Register_Files.md` | `Reg_files` | Holds the 32 general-purpose registers |
| `ALU.md` | `alu` | Executes arithmetic and logic operations |
| `ALU_Decoder.md` | `alu_decoder` | Converts instruction function fields into ALU control signals |
| `Main_Decoder.md` | `main_decoder` | Generates high-level control signals from the opcode |
| `Control_Unit_Top.md` | `Control_Unit_Top` | Connects the main decoder and ALU decoder |
| `Data_Mem.md` | `Data_Mem` | Handles data reads and writes for load/store instructions |

## Supported Behavior

From the current decoder logic, the design is aimed at a small subset of RISC-V instruction classes:

- load instructions
- store instructions
- branch-equal style control flow using the `zero` flag
- register-register ALU operations
- immediate ALU operations

The ALU currently supports:

- `ADD`
- `SUB`
- `AND`
- `OR`
- `SLT`

## Important Design Notes

- The processor is single-cycle, so every instruction completes in one clock period.
- The reset used by the program counter is active-low.
- The register file keeps register `x0` hard-wired to zero.
- Instruction memory and data memory use word addressing internally via `A[11:2]`.
- The memory blocks include simple initialization values for demonstration.
- The repository currently stores Verilog inside Markdown files instead of standalone `.v` files.

## Memory/Register Initialization

Some modules include initial values for easier simulation:

- instruction memory preloads one instruction at address `0`
- data memory preloads one word at location `28`
- register file preloads:
  - `x5 = 5`
  - `x6 = 4`

## How To Simulate

Since the Verilog is embedded inside Markdown files, the code should first be extracted into `.v` files before compiling with a simulator such as `iverilog`.

Example flow:

```sh
tmpdir=$(mktemp -d)
for f in *.md; do
  awk '/^```verilog/{flag=1;next}/^```/{flag=0}flag' "$f" > "$tmpdir/${f%.md}.v"
done
iverilog -g2012 -t null "$tmpdir"/*.v
```

This checks that the extracted Verilog compiles successfully.



## Summary

This repository is a compact educational implementation of a single-cycle RISC-V core built from small Verilog modules.
