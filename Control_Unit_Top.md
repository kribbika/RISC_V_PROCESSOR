# Control Unit Top

```verilog
`timescale 1ns / 1ps

module Control_Unit_Top(
    Op,
    zero,
    RegWrite,
    Immsrc,
    ALUSrc,
    MemWrite,
    ResultSrc,
    PCSrc,
    funct3,
    funct7,
    ALUcontrol
);
input zero;
input [6:0] Op, funct7;
input [2:0] funct3;
output RegWrite, ALUSrc, ResultSrc, PCSrc, MemWrite;
output [1:0] Immsrc;
output [2:0] ALUcontrol;

wire [1:0] ALUOp;

alu_decoder alupart(
    .ALUOp(ALUOp),
    .op5(Op[5]),
    .funct3(funct3),
    .funct7b5(funct7[5]),
    .control(ALUcontrol)
);

main_decoder mainpart(
    .op(Op),
    .zero(zero),
    .RegWrite(RegWrite),
    .Memwrite(MemWrite),
    .ResultSrc(ResultSrc),
    .ALUSrc(ALUSrc),
    .ImmSrc(Immsrc),
    .ALUOp(ALUOp),
    .PCSrc(PCSrc)
);

endmodule
```
