# Clock

You are provided a module with the following declaration:

module dut ( input clk ) ;
Write a testbench that creates one instance of module dut (with any instance name), and create a clock signal to drive the module's clk input. The clock has a period of 10 ps. The clock should be initialized to zero with its first transition being 0 to 1.
![](Images/Screen%20Shot%202022-09-29%20at%209.08.33%20pm.png)

## Solution

```verilog
module top_module ( );
    reg clk;  // use reg to define clk signal
    // initial will only run once
    initial begin
      clk = 1'd0;
    end

    // always + event , always will free run the event
    always #5 clk = ~ clk;

    // create the dut instance as the requirements
    dut dut_instance(clk);

endmodule

```

# Test bench 1

Create a Verilog testbench that will produce the following waveform for outputs A and B:
![](/HDLbits-writeups/Images/Screen%20Shot%202022-10-05%20at%2011.22.44%20pm.png)

## Solution

```verilog
module top_module ( output reg A, output reg B );//

    // generate input patterns here
    initial begin
		A = 1'd0;
        B = 1'd0;
        #10
        A = 1'd1;
        #5
        B = 1'd1;
        #5
        A = 1'd0;
        #20
        B = 1'd0;
    end

endmodule

```

# AND gate

TODO:
add graphs

## initial wrong solution

```verilog
module top_module();
    // instantiate
    andgate andgate_instance(in, out);
    initial begin
        in[1] = 1'd0;
        in[0] = 1'd0;
        out = 1'd0;
    end
    #5
    in[0] = 1'd1;
    #5
    in[0] = 1'd0;
    in[1] = 1'd1;
    #5
    in[0] = 1'd1;
    out = 1'd1;

endmodule
```

don't instantiate first. should first create all the parameters and vars and feed into the andgate instance.
More importantly ! While writing test cases, think about wat needs to be tested instead of drawing out the graph. Instead, we should input all 4 possible combination of the and gate and inspect if the output outputs the correct results.

## Modified Solution

```verilog
module top_module();
    reg [1:0] in;
    reg out;

    initial begin
        in = 'd0;
        #10
        in[1] = 1'd0;
        in[0] = 1'd1;
        #10
        in[1] = 1'd1;
        in[0] = 1'd0;
        #10
        in[1] = 1'd1;
        in[0] = 1'd1;
    end

    andgate andgate_instance(in, out);

endmodule
```

# Testbench 2

TODO: add description and graphs

```verilog
module top_module();
    // declaration
    reg clk;
    reg in;
    reg [2:0] s;
    wire out;
	initial begin
        clk = 0;
        in = 0;
        s = 3'd2;
        #10
        s = 3'd6;
        #10
        s = 3'd2;
        in = 1'd1;
        #10
        s = 3'd7;
        in = 1'd0;
        #10
        s = 3'd0;
        in = 1'd1;
        #30
        in = 1'd0;
    end
    always #5 clk = ~clk;
    q7 q7_instance (clk, in, s, out);

endmodule

```

# T flip-flop

## Recap

The toggle, or T, flip-flop is a two-input flip-flop. The inputs are the toggle (T) input and a clock (CLK) input. If the toggle input is HIGH, the T flip-flop changes state (toggles) when the clock signal is applied. If the toggle input is LOW, the T flip-flop holds the previous state.

```verilog
module top_module ();
	// prepare the testing input
    reg clk, reset, t;
    wire q;
    initial begin
        clk = 1'd0;
        reset = 1'd0;
        t = 1'd0;
        #5
        reset = 1'd1;
        #5
        t =1'd1;
        reset = 1'd0;

    end
    always #5 clk = ~clk;

    // instantiate
    tff tff_instance(clk, reset, t, q);
endmodule

```
