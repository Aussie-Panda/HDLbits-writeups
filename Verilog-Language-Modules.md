# Module

By now, you're familiar with a module, which is a circuit that interacts with its outside through input and output ports. Larger, more complex circuits are built by composing bigger modules out of smaller modules and other pieces (such as assign statements and always blocks) connected together. This forms a hierarchy, as modules can contain instances of other modules.

The figure below shows a very simple circuit with a sub-module. In this exercise, create one instance of module mod_a, then connect the module's three pins (in1, in2, and out) to your top-level module's three ports (wires a, b, and out). The module mod_a is provided for you — you must instantiate it.

When connecting modules, only the ports on the module are important. You do not need to know the code inside the module. The code for module mod_a looks like this:

```verilog
module mod_a ( input in1, input in2, output out );
    // Module body
endmodule
```

![](Images/Module_moda.png)
The hierarchy of modules is created by instantiating one module inside another, as long as all of the modules used belong to the same project (so the compiler knows where to find the module). The code for one module is not written inside another module's body (Code for different modules are not nested).

You may connect signals to the module by port name or port position. For extra practice, try both methods.
![](Images/Module.png)

## Connecting Signals to Module Ports

There are two commonly-used methods to connect a wire to a port: by position or by name.

### By position

The syntax to connect wires to ports by position should be familiar, as it uses a C-like syntax. When instantiating a module, ports are connected left to right according to the module's declaration. For example:

mod_a instance1 ( wa, wb, wc );

This instantiates a module of type mod_a and gives it an instance name of "instance1", then connects signal wa (outside the new module) to the first port (in1) of the new module, wb to the second port (in2), and wc to the third port (out). One drawback of this syntax is that if the module's port list changes, all instantiations of the module will also need to be found and changed to match the new module.

### By name

Connecting signals to a module's ports by name allows wires to remain correctly connected even if the port list changes. This syntax is more verbose, however.

mod_a instance2 ( .out(wc), .in1(wa), .in2(wb) );

The above line instantiates a module of type mod_a named "instance2", then connects signal wa (outside the module) to the port named in1, wb to the port named in2, and wc to the port named out. Notice how the ordering of ports is irrelevant here because the connection will be made to the correct name, regardless of its position in the sub-module's port list. Also notice the period immediately preceding the port name in this syntax.

```verilog
module top_module (
	input a,
	input b,
	output out
);

	// Create an instance of "mod_a" named "inst1", and connect ports by name:
	mod_a inst1 (
		.in1(a), 	// Port"in1"connects to wire "a"
		.in2(b),	// Port "in2" connects to wire "b"
		.out(out)	// Port "out" connects to wire "out"
				// (Note: mod_a's port "out" is not related to top_module's wire "out".
				// It is simply coincidence that they have the same name)
	);

/*
	// Create an instance of "mod_a" named "inst2", and connect ports by position:
	mod_a inst2 ( a, b, out );	// The three wires are connected to ports in1, in2, and out, respectively.
*/

endmodule
```

# Connecting ports by position

This problem is similar to the previous one (module). You are given a module named mod_a that has 2 outputs and 4 inputs, in that order. You must connect the 6 ports by position to your top-level module's ports out1, out2, a, b, c, and d, in that order.

You are given the following module:

module mod_a ( output, output, input, input, input, input );
![](Images/Module_name.png)

## Solutions:

```verilog
module top_module (
  input a,
  input b,
  input c,
  input d,
  output out1,
  output out2
);
  mod_a inst1 (out1,out2,a,b,c,d);
endmodule
```

# Connecting ports by name

This problem is similar to module. You are given a module named mod_a that has 2 outputs and 4 inputs, in some order. You must connect the 6 ports by name to your top-level module's ports:
You are given the following module:

module mod_a ( output out1, output out2, input in1, input in2, input in3, input in4);
![](Images/Module_name.png)

```verilog
module top_module (
    input a,
    input b,
    input c,
    input d,
    output out1,
    output out2
);
    mod_a inst1 (
      	.in1(a),
      	.in2(b),
      	.in3(c),
      	.in4(d),
      	.out1(out1),
      	.out2(out2));

endmodule
```

# Three modules

You are given a module my_dff with two inputs and one output (that implements a D flip-flop). Instantiate three of them, then chain them together to make a shift register of length 3. The clk port needs to be connected to all instances.

The module provided to you is: module my_dff ( input clk, input d, output q );

Note that to **make the internal connections, you will need to declare some wires.** Be careful about naming your wires and module instances: the names must be unique.
![](Images/Module_shift.png)

## Solutions:

```verilog
module top_module ( input clk, input d, output q );

    wire a, b;

	my_dff dff1 ( .clk(clk), .d(d), .q(a));
	my_dff dff2 ( .clk(clk), .d(a), .q(b));
	my_dff dff3 ( .clk(clk), .d(b), .q(q));
endmodule

```

# Modules and vectors

This exercise is an extension of module_shift. Instead of module ports being only single pins, we now have modules with vectors as ports, to which you will attach wire vectors instead of plain wires. Like everywhere else in Verilog, the vector length of the port does not have to match the wire connecting to it, but this will cause zero-padding or trucation of the vector. This exercise does not use connections with mismatched vector lengths.

You are given a module my_dff8 with two inputs and one output (that implements a set of 8 D flip-flops). Instantiate three of them, then chain them together to make a 8-bit wide shift register of length 3. In addition, create a 4-to-1 multiplexer (not provided) that chooses what to output depending on sel[1:0]: The value at the input d, after the first, after the second, or after the third D flip-flop. (Essentially, sel selects how many cycles to delay the input, from zero to three clock cycles.)

The module provided to you is: module my_dff8 ( input clk, input [7:0] d, output [7:0] q );

The multiplexer is not provided. One possible way to write one is inside an always block with a case statement inside. (See also: mux9to1v)
z
