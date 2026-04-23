# Program Counter

```verilog
`timescale 1ns / 1ps

module P_C(PC, PC_NEXT, rst, clk);
input [31:0] PC_NEXT;
output reg [31:0] PC;
input rst, clk;

always @(posedge clk or negedge rst) begin
    if (rst == 1'b0) begin
        PC <= 32'd0;
    end else begin
        PC <= PC_NEXT;
    end
end

endmodule
```
