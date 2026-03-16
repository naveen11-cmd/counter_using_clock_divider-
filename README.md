# counter_using_clock_divider-
# EXPERIMENT – 3.B 4- bit Up/Down Counter and MOD-10 Counter using Clock Divider in FPGA

# Aim
To design and simulate a 4-bit Up/Down Counter and MOD-10 Counter using Verilog HDL, and verify their functionality using a Clock Divider in FPGA (Vivado 2023.1).

# Apparatus Required
Vivado 2023.1

# Procedure
Launch Vivado 2023.1
Open Vivado and create a new RTL project.
Design the Verilog Code
Write the Verilog code for:
4-bit Up/Down Counter
MOD-10 Counter
Clock Divider
Create the Testbench
Write a testbench to simulate counter behavior.
The testbench should apply clock and reset signals and monitor output.
Create the Verilog Files
Add design module and testbench into the Vivado project.
Run Simulation
Run behavioral simulation.
Observe the Waveforms
Verify counting sequence:
Up counter: 0 → 15
Down counter: 15 → 0
MOD-10: 0 → 9 → 0
Save and Document Results
Capture waveform screenshots and save logs.
NOTE: These files go in Design Sources:
   clock_divider.v
   up_down_counter.v
   mod10_counter.v
   top_counter.v  (Your Top Module)
So the Top Module (Using Clock Divider) must be placed in: Design Sources
After adding it: Right click top_counter- Select Set as Top
tb_counter.v - Testbench is used only for simulation.

# Code
# Clock Divider 
```
module clk_div(
    input clk,
    input rst,
    output reg slow_clk
);

reg [3:0] count;

always @(posedge clk or posedge rst)
begin
    if(rst)
    begin
        count <= 0;
        slow_clk <= 0;
    end
    else
    begin
        count <= count + 1;

        if(count == 4)
        begin
            slow_clk <= ~slow_clk;
            count <= 0;
        end
    end
end

endmodule
```
# 4-bit Up/Down Counter
```
module up_down_counter(
    input clk,
    input rst,
    input mode,
    output reg [3:0] updown_out
);

always @(posedge clk or posedge rst)
begin
    if(rst)
        updown_out <= 0;

    else if(mode == 0)
        updown_out <= updown_out + 1;

    else
        updown_out <= updown_out - 1;
end

endmodule
```
# MOD-10 Counter
```
module mod10_counter(
    input clk,
    input rst,
    output reg [3:0] mod10_out
);

always @(posedge clk or posedge rst)
begin
    if(rst)
        mod10_out <= 0;

    else if(mod10_out == 9)
        mod10_out <= 0;

    else
        mod10_out <= mod10_out + 1;
end

endmodule
```
# Top Module (Using Clock Divider)
```
module top_counter(
    input clk,
    input rst,
    input mode,
    output [3:0] updown_out,
    output [3:0] mod10_out,
    output slow_clk
);

clk_div u1(clk,rst,slow_clk);

up_down_counter u2(
    slow_clk,
    rst,
    mode,
    updown_out
);

mod10_counter u3(
    slow_clk,
    rst,
    mod10_out
);

endmodule
```
# Testbench

```
module top_counter_tb;

reg clk;
reg rst;
reg mode;

wire [3:0] updown_out;
wire [3:0] mod10_out;
wire slow_clk;

top_counter uut(
    clk,
    rst,
    mode,
    updown_out,
    mod10_out,
    slow_clk
);

always #5 clk = ~clk;

initial
begin
    clk = 0;
    rst = 1;
    mode = 0;

    #20 rst = 0;

    #100 mode = 1;

    #200 $stop;
end

endmodule
```
# Output Waveform:
<img width="1919" height="1079" alt="Image" src="https://github.com/user-attachments/assets/e0c139e6-81e3-4171-bea8-b4861ac7238f" />
# Conclusion
The 4-bit Up/Down Counter and MOD-10 Counter were successfully designed using Verilog HDL and verified through simulation in Vivado 2023.1. The clock divider was used to generate a slower clock suitable for FPGA implementation. The waveform analysis confirmed correct counting sequences in both up/down and MOD-10 modes.
