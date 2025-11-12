# **EXP4: 4-bit Ripple Carry Adder using Task and 4-bit Ripple Counter using Function with Testbench**

---

## **Aim**
To design, simulate, and verify a **4-bit Ripple Carry Adder** using **Task** and a **4-bit Ripple Counter** using **Function** in Verilog HDL.

---

## **Apparatus Required**
- System with **Vivado Design Suite**
---

## **Theory**
### **Ripple Carry Adder**
A 4-bit Ripple Carry Adder (RCA) adds two 4-bit binary numbers by cascading four full adders. The carry-out of each full adder acts as the carry-in for the next stage. Using a **task** in Verilog, the addition operation can be modularized for each bit.
<img width="692" height="268" alt="image" src="https://github.com/user-attachments/assets/b70c348a-049a-4462-88a4-ad6905446cf4" />


### **Ripple Counter**
A Ripple Counter is a sequential circuit that counts in binary. In a 4-bit ripple counter, the output of one flip-flop serves as the clock input for the next flip-flop. Using a **function** in Verilog allows the counting logic to be encapsulated and reused.

<img width="1024" height="768" alt="image" src="https://github.com/user-attachments/assets/fa8730fc-e48e-477a-b2e3-6697a95355d3" />

---

## **Program**

### **4-bit Ripple Carry Adder using Task**

```verilog
// 4-bit Ripple Carry Adder using Task
module RA (
input [3:0] A, B,
input Cin,
output reg [3:0] Sum,
output reg Cout
);
reg c;          
integer i;
task full_adder;
    input a, b, cin;
    output s, cout;
    begin
        s = a ^ b ^ cin;                  
        cout = (a & b) | (b & cin) | (a & cin); 
    end
endtask
always @(*) begin
    c = Cin; 
    for (i = 0; i < 4; i = i + 1) begin
        full_adder(A[i], B[i], c, Sum[i], c);
    end
    Cout = c; 
end
endmodule
```

### **Test bench 4-bit Ripple Carry Adder using Task**
```
`timescale 1ns/1ps
module tb_ripple_adder_task;
reg [3:0] A, B;
reg Cin;
wire [3:0] Sum;
wire Cout;
RA uut (A, B, Cin, Sum, Cout);

initial begin
    $display("Time\tA\tB\tCin\t|\tSum\tCout");
    $monitor("%0t\t%b\t%b\t%b\t|\t%b\t%b", $time, A, B, Cin, Sum, Cout);
    A = 4'b0000; B = 4'b0000; Cin = 0; #10;
    A = 4'b0011; B = 4'b0101; Cin = 0; #10; 
    A = 4'b1111; B = 4'b0001; Cin = 0; #10;  
    A = 4'b1010; B = 4'b0101; Cin = 1; #10;  
    A = 4'b0110; B = 4'b0011; Cin = 0; #10; 
    $finish;
end
endmodule
```
### 4-bit Ripple Carry Adder Simulation Output 

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/0bd296bf-6885-430a-afb9-9cd00076506f" />


### **4-bit Ripple Counter using Function**
```
module RC (
    input clk, rst,
    output reg [3:0] Q
);
    function [3:0] count;
        input [3:0] x;
        begin
            count = x + 1;   
        end
    endfunction
    always @(posedge clk or posedge rst) begin
    if (rst)
        Q <= 4'b0000;     
    else
        Q <= count(Q);   
    end
endmodule
```
### **Testbench for 4-bit Ripple Counter using Function**
```
`timescale 1ns/1ps    
module tb_RC_func;
reg clk, rst;      
wire [3:0] Q;      
RC uut (clk,rst,Q);
always #5 clk = ~clk;
initial begin
    $display("Time(ns)\tRST\tQ");
    $monitor("%0t\t%b\t%b", $time, rst, Q);
    clk = 0;
    rst = 1;       
    #10;
    rst = 0;        
    #160;
    rst = 1;
    #10;
    rst = 0;

    #50;
    $finish;    
end
endmodule
```
### 4-bit Ripple Counter Simulation output 

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/8fb25390-3b93-4f13-8fcc-82c41a623306" />

### Result

The simulation of the 4-bit Ripple Carry Adder using Task and 4-bit Ripple Counter using Function was successfully carried out in Vivado Design Suite.
Both designs produced correct functional outputs as verified by waveform and console output.
