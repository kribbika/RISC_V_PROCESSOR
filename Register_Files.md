# Register Files

```verilog
`timescale 1ns / 1ps

module Reg_files(A1, A2, A3, WD3, WE3, clk, rst, RD1, RD2);
input [4:0] A1, A2, A3;
input [31:0] WD3;
input clk, rst, WE3;
output [31:0] RD1, RD2;

reg [31:0] Registers[31:0];
integer i;

assign RD1 = ((rst == 1'b0) || (A1 == 5'd0)) ? 32'd0 : Registers[A1];
assign RD2 = ((rst == 1'b0) || (A2 == 5'd0)) ? 32'd0 : Registers[A2];

always @(posedge clk) begin
    if ((rst == 1'b1) && (WE3 == 1'b1) && (A3 != 5'd0)) begin
        Registers[A3] <= WD3;
    end
end

initial begin
    for (i = 0; i < 32; i = i + 1) begin
        Registers[i] = 32'd0;
    end

    Registers[5] = 32'h00000005;
    Registers[6] = 32'h00000004;
end

endmodule
```
