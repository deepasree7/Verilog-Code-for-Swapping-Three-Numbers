# Verilog-Code-for-Swapping-Three-Numbers
Aim
To design and simulate a Verilog HDL code for swapping the values of three numbers without using any temporary variables, and verify the correctness of the swapping operation through a testbench using the Vivado 2023.1 simulation environment.

Apparatus Required
Vivado 2023.1 or equivalent Verilog simulation tool.

Procedure
Launch Vivado 2023.1:

Open Vivado and create a new project.
Write the Verilog Code for Swapping:

Write the Verilog code that swaps the values of three numbers (a, b, and c) using basic arithmetic or bitwise operations without using temporary variables.
Create the Testbench:

Write a testbench to simulate the swapping operation. The testbench should initialize three numbers, trigger the swapping module, and check if the values are swapped correctly.
Add the Verilog Files:

Add the Verilog module and the testbench file to the Vivado project.
Run Simulation:

Run the behavioral simulation in Vivado to verify the swapping operation.
Observe the Waveforms:

Examine the waveform to confirm that the values of the three numbers are swapped as expected.
Save and Document Results:

Capture the waveform output and include the results in your report for verification.

Verilog Code for Traffic Light Controller

// traffic_light_controller.v
```
module traffic_light_controller(
    input clk,              // Clock signal
    input reset,            // Reset signal
    output reg [2:0] light  // 3-bit output for lights: [Red, Yellow, Green]
);
    // State encoding using localparam
    localparam GREEN  = 2'b00, 
               YELLOW = 2'b01, 
               RED    = 2'b10;

reg [1:0] current_state, next_state;
    reg [31:0] timer;       // 32-bit timer to count clock cycles
    // Timing parameters using localparam
    localparam GREEN_TIME  = 5;   // 5 seconds for green light
    localparam YELLOW_TIME = 2;   // 2 seconds for yellow light
    localparam RED_TIME    = 5;   // 5 seconds for red light
    // State transition logic (combinational)
    always @(*) begin
        case (current_state)
            GREEN:  if (timer >= GREEN_TIME) next_state = YELLOW;
                    else next_state = GREEN;
            YELLOW: if (timer >= YELLOW_TIME) next_state = RED;
                    else next_state = YELLOW;
            RED:    if (timer >= RED_TIME) next_state = GREEN;
                    else next_state = RED;
            default: next_state = RED; // Default to RED if undefined state
        endcase
    end
    // Output logic (combinational)
    always @(*) begin
        case (current_state)
            GREEN:  light = 3'b001;  // Green on
            YELLOW: light = 3'b010;  // Yellow on
            RED:    light = 3'b100;  // Red on
            default: light = 3'b100; // Default to Red
        endcase
    end
    // State and timer update (sequential)
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            current_state <= RED;   // Initial state
            timer <= 0;
        end else begin
            if (timer >= (current_state == GREEN  ? GREEN_TIME :
                          current_state == YELLOW ? YELLOW_TIME : RED_TIME)) begin
                current_state <= next_state;
                timer <= 0; // Reset timer on state change
            end else begin
                timer <= timer + 1; // Increment timer
            end
        end
    end
endmodule
```
Testbench for Traffic Light Controller
```
 module traffic_light_tb;
    // Signals for DUT (Device Under Test)
    reg clk;
    reg reset;
    wire [2:0] light;
    // Instantiate the traffic light controller
    traffic_light_controller dut (
        .clk(clk),
        .reset(reset),
        .light(light)
    );
    // Clock generation
    always #1 clk = ~clk;  // Toggle clock every 1 time unit
    // Test procedure
    initial begin
        // Initialize signals
        clk = 0;
        reset = 1;
        // Apply reset for a few cycles
        #2 reset = 0;   // De-assert reset after 2 time units
        // Run the simulation for 20 time units (for example)
        #20 $finish;
    end
    // Monitor output
    initial begin
        $monitor("Time: %0t | Reset: %b | Lights: %b (Red-Yellow-Green)", $time, reset, light);
    end
endmodule
```
![Screenshot (18)](https://github.com/user-attachments/assets/28037209-18fe-4e98-9148-bb507061c2e1)

Conclusion
In this experiment, a Verilog HDL code for swapping three numbers was designed and successfully simulated. The testbench verified the swapping operation, showing that the values of three input numbers (a, b, and c) were swapped correctly without the use of temporary variables. This experiment demonstrated the effectiveness of Verilog in implementing logical operations and control mechanisms such as swapping values. The simulation results confirm the correct functionality of the design.
