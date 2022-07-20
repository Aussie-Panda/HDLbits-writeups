# Module Declaration

## Getting started

We're going to start with a small bit of HDL to get familiar with the interface used by HDLBits. Here's the description of the circuit you need to build for this exercise:

Build a circuit with no inputs and one output. That output should always drive 1 (or logic high).

```verilog
// <size>'<radix><value>
// 2'b01 size 2 binary, value is 1
module top_module( output one );

  // Insert your code here
  assign one = 1'b1;
ß
endmodule

```

## Zero

Build a circuit with no inputs and one output that outputs a constant 0

Now that you've worked through the previous problem, let's see if you can do a simple problem without the hints.

```verilog
module top_module ( output zero );

	assign zero = 1'b0;

endmodule
```
