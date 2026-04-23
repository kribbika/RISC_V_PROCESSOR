# Data Memory

```verilog
`timescale 1ns / 1ps

module Data_Mem(A, WD, clk, WE, RD, rst);
input [31:0] A, WD;
input clk, WE, rst;
output [31:0] RD;

reg [31:0] Data_MEM[1023:0];
wire [9:0] word_addr;

assign word_addr = A[11:2];
assign RD = (rst == 1'b0) ? 32'd0 : Data_MEM[word_addr];

always @(posedge clk) begin
    if (WE == 1'b1) begin
        Data_MEM[word_addr] <= WD;
    end
end

initial begin
    Data_MEM[28] = 32'h00000020;
end

endmodule
```
