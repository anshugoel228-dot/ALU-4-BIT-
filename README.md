# ALU-4-BIT code
I recently designed and simulated an 8-bit Arithmetic Logic Unit (ALU) using Verilog HDL, focusing on core digital design and RTL concepts. This project implements multiple arithmetic and logical operations such as addition, subtraction, AND, OR, XOR, NOT, and shift operations, all controlled through a 4-bit select line. I developed the complete RTL architecture using behavioral modeling (always block with case statements), ensuring modular and synthesizable code. A dedicated testbench was created to verify functionality across all operations, and waveform analysis was used to validate outputs and carry behavior. Additionally, I explored the internal structure through schematic-level representation, giving a deeper understanding of how logic gates and multiplexers work together to form the ALU. This project strengthened my skills in Verilog coding, simulation, debugging, and digital system design, bridging the gap between theoretical concepts and practical hardware implementation.




#MODULE
module alu(
    input  [7:0] A, B,
    input  [3:0] ALU_Sel,
    output [7:0] ALU_Out,
    output CarryOut
);

reg [7:0] ALU_Result;
wire [8:0] tmp;

assign ALU_Out = ALU_Result;

// For carry calculation
assign tmp = {1'b0, A} + {1'b0, B};
assign CarryOut = tmp[8];

always @(*) begin
    case (ALU_Sel)
        4'b0000: ALU_Result = A + B;   // Addition
        4'b0001: ALU_Result = A - B;   // Subtraction
        4'b0010: ALU_Result = A & B;   // AND
        4'b0011: ALU_Result = A | B;   // OR
        4'b0100: ALU_Result = A ^ B;   // XOR
        4'b0101: ALU_Result = ~(A | B);// NOR
        4'b0110: ALU_Result = A << 1;  // Left Shift
        4'b0111: ALU_Result = A >> 1;  // Right Shift
        default: ALU_Result = 8'b0;
    endcase
end

endmodule



# TESTBENCH


`timescale 1ns/1ps

module alu_tb;

reg [7:0] A, B;
reg [3:0] ALU_Sel;
wire [7:0] ALU_Out;
wire CarryOut;

integer i;

// Instantiate ALU
alu test_unit (
    .A(A),
    .B(B),
    .ALU_Sel(ALU_Sel),
    .ALU_Out(ALU_Out),
    .CarryOut(CarryOut)
);
  
  initial begin
    $dumpfile("wave.vcd"); // FOR EDA TOOL BACKGROUND
    $dumpvars(0, alu_tb);   
end


initial begin
    A = 8'h05;
    B = 8'h04;
    ALU_Sel = 4'h0;

    for (i = 0; i < 8; i = i + 1) begin
        #10 ALU_Sel = ALU_Sel + 1;
    end

    #20 $finish;

  end
endmodule
