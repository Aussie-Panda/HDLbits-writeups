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

which is obviously wrong. What we should do is to invert the output instead of the whole instance. Declare a wire to store the temp value as well as additional not gate

**Also, remember to set unused port for and gate to 1**

```verilog
module top_module (input a, input b, input c, output out);//
	wire out1;
    andgate inst1 ( .out(out1), .a(a), .b(b), .c(c), .d(1'b1), .e(1'b1)  );
	assign out = ~out1;
endmodule
```

# Bugs mux4

## Problem

This 4-to-1 multiplexer doesn't work. Fix the bug(s).

You are provided with a bug-free 2-to-1 multiplexer:

module mux2 (
input sel,
input [7:0] a,
input [7:0] b,
output [7:0] out
);

```verilog
module top_module (
    input [1:0] sel,
    input [7:0] a,
    input [7:0] b,
    input [7:0] c,
    input [7:0] d,
    output [7:0] out  ); //

    wire mux0, mux1;
    mux2 mux0 ( sel[0],    a,    b, mux0 );
    mux2 mux1 ( sel[1],    c,    d, mux1 );
    mux2 mux2 ( sel[1], mux0, mux1,  out );

endmodule

```

## Solution

The wire should be defined as the same length as output. Using 2 mux to create one 4 mux should be both sel[0] on the level 1.

```verilog
    input [1:0] sel,
    input [7:0] a,
    input [7:0] b,
    input [7:0] c,
    input [7:0] d,
    output [7:0] out  ); //

    wire [7:0]mux0, mux1;
    mux2 mux_0 ( sel[0],    a,    b, mux0 );
    mux2 mux_1 ( sel[0],    c,    d, mux1 );
    mux2 mux_2 ( sel[1], mux0, mux1,  out );

endmodule

```

# Bugs addsubz

The following adder-subtractor with zero flag doesn't work. Fix the bug(s).

## Problem

```verilog
// synthesis verilog_input_version verilog_2001
module top_module (
    input do_sub,
    input [7:0] a,
    input [7:0] b,
    output reg [7:0] out,
    output reg result_is_zero
);//

    always @(*) begin
        case (do_sub)
          0: out = a+b;
          1: out = a-b;
        endcase

        if (~out)
            result_is_zero = 1;
    end

endmodule

```

## Solution

added default case for extra safe and else case to avoid latches

```verilog
// synthesis verilog_input_version verilog_2001
module top_module (
    input do_sub,
    input [7:0] a,
    input [7:0] b,
    output reg [7:0] out,
    output reg result_is_zero
);//

    always @(*) begin
        case (do_sub)
          0: out = a+b;
          1: out = a-b;
          default
            out = 0;
        endcase

        if (out== 8'd0)
            result_is_zero = 1;
        else
            result_is_zero = 0;
    end

endmodule

```

# Bugs case

This combinational circuit is supposed to recognize 8-bit keyboard scancodes for keys 0 through 9. It should indicate whether one of the 10 cases were recognized (valid), and if so, which key was detected. Fix the bug(s).

## Problem

```verilog
module top_module (
    input [7:0] code,
    output reg [3:0] out,
    output reg valid=1 );//

     always @(*)
        case (code)
            8'h45: out = 0;
            8'h16: out = 1;
            8'h1e: out = 2;
            8'd26: out = 3;
            8'h25: out = 4;
            8'h2e: out = 5;
            8'h36: out = 6;
            8'h3d: out = 7;
            8'h3e: out = 8;
            6'h46: out = 9;
            default: valid = 0;
        endcase

endmodule

```

## Solution

add assignment in the beginning to avoid latches and missing cases

```verilog
module top_module (
    input [7:0] code,
    output reg [3:0] out,
    output reg valid=1 );//

    always @(*) begin
      out = 0;
    	valid = 1;
          case (code)
            8'h45: out = 0;
            8'h16: out = 1;
            8'h1e: out = 2;
            8'h26: out = 3;
            8'h25: out = 4;
            8'h2e: out = 5;
            8'h36: out = 6;
            8'h3d: out = 7;
            8'h3e: out = 8;
            8'h46: out = 9;
            default: valid = 0;
        endcase
    end

endmodule

```
