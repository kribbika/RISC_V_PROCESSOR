# ALU Decoder

```verilog
`timescale 1ns / 1ps

module alu_decoder(ALUOp, op5, funct3, funct7b5, control);
input op5, funct7b5;
input [2:0] funct3;
input [1:0] ALUOp;
output reg [2:0] control;

localparam [2:0] ALU_ADD = 3'b000;
localparam [2:0] ALU_SUB = 3'b001;
localparam [2:0] ALU_AND = 3'b010;
localparam [2:0] ALU_OR  = 3'b011;
localparam [2:0] ALU_SLT = 3'b101;

always @(*) begin
    control = ALU_ADD;

    case (ALUOp)
        2'b00: control = ALU_ADD;
        2'b01: control = ALU_SUB;
        2'b10: begin
            case (funct3)
                3'b000: control = ({op5, funct7b5} == 2'b11) ? ALU_SUB : ALU_ADD;
                3'b010: control = ALU_SLT;
                3'b110: control = ALU_OR;
                3'b111: control = ALU_AND;
                default: control = ALU_ADD;
            endcase
        end
        default: control = ALU_ADD;
    endcase
end

endmodule
```
