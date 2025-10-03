# 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task

# Aim
To design and simulate a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required
Vivado 2023.1
# Procedure
1. Launch Vivado 2023.1
Open Vivado and create a new project.
2. Design the Verilog Code
Write the Verilog code for the seven-segment display, defining the logic that maps a 4-bit binary input to the corresponding segments (a to g) of the display.
3. Create the Testbench
Write a testbench to simulate the seven-segment display behavior. The testbench should apply various 4-bit input values and monitor the corresponding output.
4. Create the Verilog Files
Create both the design module and the testbench in the Vivado project.
5. Run Simulation
Run the behavioral simulation to verify the output. 
6. Observe the Waveforms
Analyze the output waveforms in the simulation window, and verify that the correct segments light up for each digit.
7. Save and Document Results
Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Verilog Code
# 4 bit Ripple Adder using Task
```verilog
module ripple_adder_task (
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

    always @(*) 
    begin
        c = Cin;
        for (i = 0; i < 4; i = i + 1) begin
            full_adder(A[i], B[i], c, Sum[i], c);
        end
        Cout = c;
    end
endmodule
```
# Test Bench

```verilog
module ripple_adder_tb;
    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    ripple_adder_task dut (
        .A(A), .B(B), .Cin(Cin), .Sum(Sum), .Cout(Cout)
    );

    initial begin
        $monitor("Time=%0t | A=%b B=%b Cin=%b | Sum=%b Cout=%b", 
                  $time, A, B, Cin, Sum, Cout);

        A = 4'b0011; B = 4'b0010; Cin = 1'b0;
        #10;

        A = 4'b0111; B = 4'b0101; Cin = 1'b0;
        #10;

        A = 4'b1010; B = 4'b0110; Cin = 1'b1;
        #10;

        A = 4'b1111; B = 4'b1111; Cin = 1'b0;
        #10;

        $finish;
    end
endmodule
```

# Output Waveform
<img width="867" height="632" alt="image" src="https://github.com/user-attachments/assets/87359fa9-baf1-45bc-aad3-84a15572852b" />
------------------------------------------------------
# 4 bit Ripple counter using Function
```verilog
module ripple_counter_func (
    input clk, rst,
    output reg [3:0] Q
);
    function [3:0] count (input [3:0] current_q);
        count = current_q + 1;
    endfunction

    always @(posedge clk or posedge rst) begin
        if (rst)
            Q <= 4'b0000;
        else
            Q <= count(Q);
    end
endmodule
```
# Test Bench
```verilog
module ripple_counter_tb;
    reg clk, rst;
    wire [3:0] Q;

    ripple_counter_func dut (
        .clk(clk), .rst(rst), .Q(Q)
    );

    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        $monitor("Time=%0t | rst=%b | Q=%d", $time, rst, Q);
        
        rst = 1;
        #15;

        rst = 0;
        
        #180;

        $finish;
    end
endmodule
```

# Output Waveform 
<img width="865" height="637" alt="image" src="https://github.com/user-attachments/assets/8834bcd0-a957-4f1f-8764-08cd34de6fbd" />
-------------------------------------------------------------------

# Conclusion
In this experiment, a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task was successfully designed and simulated using Verilog HDL.
