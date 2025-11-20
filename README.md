# **Simulation of Sequence Detector Using Mealy Machine in Verilog HDL**

---

## **Aim**
To design and simulate a **Sequence Detector using Mealy Machine** in Verilog HDL that detects a specific bit pattern from a given binary input sequence.

---

## **Apparatus Required**
- Computer with **Vivado Design Suite** installed  
---

## **Theory**

A **Sequence Detector** identifies a specific sequence of bits from a stream of binary input data.  
**Mealy Machine** – Output depends on both the current state and the input.

In the **Mealy Machine**, the output can change immediately in response to an input, making it faster than the Moore type.  
The Mealy model uses **state transitions** and **output logic** based on both the current state and the input signal.

For example, a **sequence detector** that detects `"1011"`:
- It produces an output `1` whenever the sequence `1011` appears in the input stream.
- Overlapping sequences are also detected.

---
## State Diagram 
<img width="363" height="550" alt="image" src="https://github.com/user-attachments/assets/7861102e-c301-422a-bfa8-d8d887a8652b" />

## Verilog Code
```
module mealysequence(clk,reset,din,dout);
 input clk;
 input reset;
 input din;
 output reg dout;
 
 parameter S0 = 3'b000,
           S1 = 3'b001,
           S2 = 3'b010,
           S3 = 3'b011,
           S4 = 3'b100;
           
 reg [2:0] current_state, next_state;
 
 
 always @(posedge clk or posedge reset) 
 begin
     if (reset) 
         current_state <= S0;
     else 
         current_state <= next_state;
 end
 
 always @(*) 
 begin
     case (current_state)
         S0: begin
                 if (din) 
                 begin
                     next_state = S1;
                     dout = 1'b0;
                 end
                 else 
                 begin
                     next_state = S0;
                     dout = 1'b0;
                 end
             end
         S1: begin
                 if (din) 
                 begin
                     next_state = S2;
                     dout = 1'b0;
                 end
                 else 
                 begin
                     next_state = S0;
                     dout = 1'b0;
                 end
             end
         S2: begin
                 if (din) 
                 begin
                     next_state = S2;  
                     dout = 1'b0;
                 end
                 else 
                 begin
                     next_state = S3;
                     dout = 1'b0;
                 end
             end
         S3: begin
                 if (din) 
                 begin
                     next_state = S4;
                     dout = 1'b0;
                 end
                 else 
                 begin
                     next_state = S0;
                     dout = 1'b0;
                 end
             end
         S4: begin
                 if (din) 
                 begin
                     next_state = S2; 
                     dout = 1'b1;      
                 end
                 else 
                 begin
                     next_state = S0;
                     dout = 1'b0;
                 end
             end
         default: begin
                     next_state = S0;
                     dout = 1'b0;
                 end
     endcase
 end
endmodule




```
## Testbench
```
`timescale 1ns/1ps
module tb_mealy;
 reg clk, reset, din;
 wire dout;
 
 mealysequence uut (clk, reset, din, dout);
 
 initial 
 begin
     clk = 0;
     forever #5 clk = ~clk; 
 end
 
 initial 
 begin
     
     reset = 1; din = 0;
     #10 reset = 0;
     
     
     #10 din = 1; 
     #10 din = 1;  
     #10 din = 0; 
     #10 din = 1; 
     
     
     
     #50 $finish;
 end
endmodule
```
## Simulation Output 
---
<img width="1666" height="974" alt="image" src="https://github.com/user-attachments/assets/5258f5cf-9574-421d-b4d8-b7396814496a" />

Paste the output here

---
## Result

The Mealy Machine Sequence Detector for the bit pattern "11011" was successfully designed and simulated using Verilog HDL.
Simulation results confirm that the circuit correctly detects the pattern and supports overlapping sequences.

