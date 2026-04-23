# Main Decoder

```verilog
`timescale 1ns / 1ps

module main_decoder(op, zero, RegWrite, Memwrite, ResultSrc, ALUSrc, ImmSrc, ALUOp, PCSrc);
input [6:0] op;
input zero;
output RegWrite, Memwrite, ResultSrc, ALUSrc, PCSrc;
output [1:0] ALUOp, ImmSrc;

localparam [6:0] OP_LOAD   = 7'b0000011;
localparam [6:0] OP_STORE  = 7'b0100011;
localparam [6:0] OP_BRANCH = 7'b1100011;
localparam [6:0] OP_IMM    = 7'b0010011;
localparam [6:0] OP_REG    = 7'b0110011;

wire is_load;
wire is_store;
wire is_branch;
wire is_imm;
wire is_reg;

assign is_load   = (op == OP_LOAD);
assign is_store  = (op == OP_STORE);
assign is_branch = (op == OP_BRANCH);
assign is_imm    = (op == OP_IMM);
assign is_reg    = (op == OP_REG);

assign RegWrite  = is_load | is_imm | is_reg;
assign ALUSrc    = is_load | is_store | is_imm;
assign PCSrc     = is_branch & zero;
assign Memwrite  = is_store;
assign ResultSrc = is_load;
assign ImmSrc    = is_branch ? 2'b10 :
                   is_store  ? 2'b01 :
                   2'b00;
assign ALUOp     = (is_reg | is_imm) ? 2'b10 :
                   is_branch          ? 2'b01 :
                   2'b00;

endmodule
```
