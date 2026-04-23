# ALU

```verilog
`timescale 1ns / 1ps

module alu(
    output [31:0] y,
    output n,
    output z,
    output o,
    output c,
    output [31:0] e,
    input [31:0] a,
    input [31:0] b,
    input [2:0] control
);

wire [31:0] a_and_b;
wire [31:0] a_or_b;
wire [31:0] not_b;
wire [31:0] mux_b;
wire [31:0] sum;
wire carry;
wire add_overflow;
wire sub_overflow;

assign not_b   = ~b;
assign a_or_b  = a | b;
assign a_and_b = a & b;
assign mux_b   = (control[0] == 1'b0) ? b : not_b;

assign {carry, sum} = a + mux_b + control[0];

assign add_overflow = ~(a[31] ^ b[31]) & (a[31] ^ sum[31]);
assign sub_overflow =  (a[31] ^ b[31]) & (a[31] ^ sum[31]);

assign o = (control == 3'b000) ? add_overflow :
           (control == 3'b001) ? sub_overflow :
           1'b0;

assign e = {31'b0, sum[31] ^ o};

assign y = (control == 3'b000) ? sum :
           (control == 3'b001) ? sum :
           (control == 3'b010) ? a_and_b :
           (control == 3'b011) ? a_or_b :
           (control == 3'b101) ? e :
           32'd0;

assign z = ~|y;
assign n = y[31];
assign c = ((control == 3'b000) || (control == 3'b001)) ? carry : 1'b0;

endmodule
```
