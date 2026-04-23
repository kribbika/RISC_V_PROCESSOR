# Instruction Memory

```verilog
`timescale 1ns / 1ps

module Instr_Mem(A, rst, RD);
input [31:0] A;
input rst;
output [31:0] RD;

reg [31:0] Mem[1023:0];
wire [9:0] word_addr;

assign word_addr = A[11:2];
assign RD = (rst == 1'b0) ? 32'd0 : Mem[word_addr];

initial begin
    Mem[0] = 32'hFFC4A303;
end

endmodule
```
