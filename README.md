# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,
![image](https://github.com/user-attachments/assets/2c844965-89f8-437f-8056-3e27fbbe1f61)


◦ Liberty Files (.lib)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)
reg clk;
reg reset;
reg up_down;
reg enable;
wire [3:0] count;


up_down_counter uut(
    .clk(clk),
    .reset(reset),
    .up_down(up_down),
    .enable(enable),
    .count(count)
);


initial begin
    clk = 0;
    forever #5 clk = ~clk;  
end


initial begin

    reset = 1;
    up_down = 1;
    enable = 0;
    

    #20 reset = 0;
    

    #10 enable = 1;
    #200;  
    

    up_down = 0;
    #200;  
    

    enable = 0;
    #50;
    

    reset = 1;
    #20 reset = 0;
    

    #50;
    

    $finish;
end


initial begin
    $monitor("Time: %t, Reset: %b, Up/Down: %b, Enable: %b, Count: %b", 
             $time, reset, up_down, enable, count);
end

##Design

    if (reset) 
    begin
        count <= 4'b0000;
    end

    else if (enable)
    begin
        if (up_down)
        begin
            count <= count + 1'b1;
        end
        else begin
            count <= count - 1'b1;
        end
    end
end

◦ SDC (Synopsis Design Constraint) File (.sdc)
![image](https://github.com/user-attachments/assets/a41a5a7f-a752-4016-a1b5-4dcf8b5c82b1)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.
![image](https://github.com/user-attachments/assets/ec2aaac7-2c95-4bc9-a274-e3eeba9c6a74)



•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.
![image](https://github.com/user-attachments/assets/f105c49d-05d3-4d53-80f3-b5503e4aa22f)

![image](https://github.com/user-attachments/assets/78598e5b-de8f-48c0-9045-d63a62c8246b)

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :

#### Area report:
![image](https://github.com/user-attachments/assets/8b250113-5e38-47e2-a2ba-0295bcf9a788)


#### Power Report
![image](https://github.com/user-attachments/assets/a4ca58fe-bd6b-43a7-9a6d-0f7362f6948b)



#### Timing Report: 
![image](https://github.com/user-attachments/assets/1dcc0b38-6264-4781-9065-cc2b63050228)


#### Result: 
![image](https://github.com/user-attachments/assets/aaf2d45b-61ba-4c73-8e3c-073c7fd48741)


The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





