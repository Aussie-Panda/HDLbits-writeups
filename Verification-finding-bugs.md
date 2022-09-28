# Bug mux2

## Problem

```verilog
module top_module (
    input sel,
    input [7:0] a,
    input [7:0] b,
    output out  );

    assign out = (~sel & a) | (sel & b);

endmodule
```

## Solution

We can first noticed that sel is 1 bit and out is 1 bit. Which means the bit-wise logic is confusing and the output size mismatch the input size. So I change the mux logic and fix the output size.

```verilog
module top_module (
    input sel,
    input [7:0] a,
    input [7:0] b,
    output [7:0] out  );

    assign out = (sel == 0) ? b : a;

endmodule
```

# Bugs nand3

This three-input NAND gate doesn't work. Fix the bug(s).

You must use the provided 5-input AND gate:

module andgate ( output out, input a, input b, input c, input d, input e );

## Problem

```verilog
module top_module (input a, input b, input c, output out);//

    andgate inst1 ( a, b, c, out );

endmodule

```

## Solution

My first instinct is to do:

```verilog
module top_module (input a, input b, input c, output out);//

    andgate inst1 ( a, b, c, out );
	assign out = ~inst1;
endmodule

```

which is obviously wrong. What we should do is to invert the output instead of the whole instance. Declare a wire to store the temp value as well as addinga not gate.

**Also, remember to set unused port for and gate to 1**

```verilog
module top_module (input a, input b, input c, output out);//
	wire out1;
    andgate inst1 ( .out(out1), .a(a), .b(b), .c(c), .d(1'b1), .e(1'b1)  );
	assign out = ~out1;
endmodule
```

#

## Problem

```verilog

```

## Solution

```verilog

```

#

## Problem

```verilog

```

## Solution

```verilog

```

#

## Problem

```verilog

```

## Solution

```verilog

```
