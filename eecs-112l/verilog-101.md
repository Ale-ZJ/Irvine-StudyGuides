# Verilog 101

## Intro

* Hardware description language can be&#x20;
  * **combinatorial**: only has logic gates
  * **sequential**: logic gates and memory elements
* Hardware description language can also be
  * **synchronous:** logic is governed by a single clock cycle
  * **asynchronous:** states self propagate&#x20;
    * race conditions
* In a design, there is a datapath, and a control path

## Module

* **module:** a design in Verilog consists of one part called the module

```
module identifier (ports_list);
    [ports_declaration]
    module_body
endmodule
```

* the ports\_list are the inputs and outputs of the module
* ports\_declaration will have any **reg** and **wire** &#x20;
  * **reg** is to store values until updates, just like a variable
    * driven in sequential statements
  * **wire** is to carry a signal through but does not store the value
    * driven in concurrent statements
  * both can be a bus, meaning multiple bits
  * both have four values
    * 0 = false
    * 1 = true
    * x = unkown
    * z = high impedance, most likely something is not connected

| combinatorial                                                                                                               | sequential                                  |
| --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| no clocks                                                                                                                   | clocks                                      |
| only `wires` and gates                                                                                                      | `registers` hold values between clock edges |
| no latch, nor memory, nor FF                                                                                                | yes FF                                      |
| use `assign` statement for concurrent statements, statements that can be executed in parallel without affecting the outcome | `=`                                         |
| `always` block to simulate sequential                                                                                       | `always` block on poseedge of clock         |

* if a module has multiple always blocks, then all of them will run in parallel
* stuff inside the always block will execute once every time that a variable in the sensitivity list changes values

## Data Types

a 4-bit vector

```
wire [3:0] areg;
```

a memory of 4 one-bit elements

```
reg amem [0:3];
```

a memory of four 6-bit elements, useful for registers

```
reg [0:5] bmem [3:0];
```

a two dimensional memory of one-bit elements

```
reg cmem [3:0] [2:0];
```

## Blocking vs Non-blocking Assignment

inside a sequential clock (like always or initial)&#x20;

* blocking&#x20;
  * \=
  * result of the assignment seen in the subsequent statement
    * evaluation and assignment are immediate
  * statements are executed sequentially
* non-blocking&#x20;
  * <=
  * results of assignment are not seen in subsequent statements
    * all assignments are deferred until all right-hand sides have been evaluated
  * concurrent execution

## Design Verification Principles

### Testbenches&#x20;

we use simulations to verify the correctness of the design. The user provides the input for the design and the simulation generated the output

* test vectors: sequence of input values&#x20;
* waveform: graphical depiction of this sequence&#x20;
* a testbench DOES NOT HAVE any input/output port

#### how to write a clock signal the pro way in verilog

```
initial begin
    clk = 0;
    forever #10 clk = ~clk;
end
```

#### how to write a clock signal the pro way in systemverilog

```
always #10 clk = ~clk;
```

we dont want to go through the waveform and check every single cycle right, so we want to write **self-checking testbench**

* generates enough stimulus&#x20;
* drive all design inputs&#x20;
* monitor all outputs&#x20;
* validate the correctness of output, predicting and checking

### SystemVerilog for Verification

* structural procedures&#x20;
  * initial procedure
    * enabled once at the beginning of a simulation&#x20;
  * always procedure
    * enabled repeatedly until the simulation is terminated&#x20;
  * final procedure
    * enabled at the end of the simulation only once
  * function
* timing control&#x20;
* assertion statements&#x20;
  * assert that an expression is true, otherwise an error is generated
  * syntax
    * optional label
    * assert expression
    * optional action block that executes immediately after the evaluation of the expression
    * optional else block
  * Label: assert (a < b) $display("ok"); else begin yada yada end;
* system functions&#x20;
  * $display : display value to monitor once every time they are executed&#x20;
  * $monitor : display value to monitor when one of its parameter changes
  * $time : return the simulation time as a 64-bit integer&#x20;
  * $finish : exits the simulator back to the os
  * $readmemh : initialize memory from a text file with hex values
* randomization&#x20;

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

