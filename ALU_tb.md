`timescale 1ns / 1ps

module alu_tb;

reg  [31:0] a, b;
reg  [2:0]  control;

wire [31:0] y;
wire n, z, o, c;
wire [31:0] e;

// Instantiate DUT
alu uut (
    .y(y),
    .n(n),
    .z(z),
    .o(o),
    .c(c),
    .e(e),
    .a(a),
    .b(b),
    .control(control)
);

// Task to display results
task display;
begin
    $display("CTRL=%b | a=%h b=%h | y=%h | n=%b z=%b c=%b o=%b | e=%h",
              control, a, b, y, n, z, c, o, e);
end
endtask

initial begin

    $display("==== ALU TEST START ====");

   
    // ADD tests
    
    control = 3'b000;

    a = 10; b = 5; #10; display();
    a = 32'h7FFFFFFF; b = 1; #10; display();   // overflow
    a = 32'hFFFFFFFF; b = 1; #10; display();   // carry

 
    // SUB tests
   
    control = 3'b001;

    a = 10; b = 5; #10; display();
    a = 5; b = 10; #10; display();             // negative result
    a = 32'h80000000; b = 1; #10; display();   // overflow

   
    // AND

    control = 3'b010;

    a = 32'hF0F0F0F0; b = 32'h0F0F0F0F; #10; display();


    // OR
  
    control = 3'b011;

    a = 32'hF0F0F0F0; b = 32'h0F0F0F0F; #10; display();


    // XOR
 
    control = 3'b100;

    a = 32'hAAAAAAAA; b = 32'h55555555; #10; display();


    // NOT A

    control = 3'b101;

    a = 32'h00000000; #10; display();


    // PASS A

    control = 3'b110;

    a = 32'h12345678; #10; display();

 
    // NOT B

    control = 3'b111;

    b = 32'hFFFFFFFF; #10; display();

   
    // Zero flag check
 
    control = 3'b000;

    a = 5; b = -5; #10; display(); // result = 0 → z=1

   
    // SLT (e output) tests
   
    control = 3'b001;

    a = 5; b = 10; #10; display();   // a < b → e=1
    a = 10; b = 5; #10; display();   // a > b → e=0

    // Edge case with overflow
    a = 32'h7FFFFFFF; b = -1; #10; display();

    $display("==== ALU TEST END ====");
    $finish;
end

endmodule
