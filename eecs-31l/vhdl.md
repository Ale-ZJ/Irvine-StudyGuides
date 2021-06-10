---
description: 'Nazeih Botros, "HDL with Digital Design VHDL AND VERILOG"'
---

# VHDL

Hardware Description Language \(**HDL**\) is a computer-aided design \(**CAD**\) used to design high-density programmable chips such as application-specific integrated circuits \(**ASICs**\) and field-programmable gate arrays \(**FPGAs**\), which are used in the design of digital systems. The two widely used hardware description languages are **VHDL** and **Verilog**.

## Structure of VHDL Module

The VHDL module consists of: `ENTITY` and `ARCHITECTURE`.

### Entity

`ENTITY` declares the input \(input ports\) and output \(output ports\) signals of the system.

```cpp
ENTITY <name> IS
    PORT ( <signal_1>, <signal_2>: IN bit;
           <signal_3>: OUT bit )
END <name>;

//Example 
ENTITY half_adder IS
    PORT (a: IN bit; b : IN bit; S : OUT bit;
          C: OUT bit);
END half_adder;
```

* `IN` instantiates the mode of the port as input
* `bit` is the _signal type_ and determines the allowed values that signals `a` and `b` can take 
  * other types: `std_logic`, `std_logic_vector`,  `real`,  `integer` 
* `;` \(semicolon\) indicates a new statement
* `END` indicates the end of the entity. You can add the name of the entity after `END` or not.

{% hint style="warning" %}
VHDL is case insensitive!
{% endhint %}

### Architecture

`ARCHITECTURE` describes the relationship between the inputs and the outputs of the system. 

{% hint style="danger" %}
`ARCHITECTURES` MUST be bounded to ONLY ONE entity, but multiple `ARCHITECTURES` can be bound to the same entity.
{% endhint %}

```cpp
ARCHITECTURE <architecture_name> OF <entity_name> IS
BEGIN
S <= a xor b; --statement 1
C <= a and b; --statement 2
--Blank lines are allowed
END dtfl_half;
```

The architecture recognizes the information declared in the entity.

* `OF` binds the architecture name with the entity name
* `<=` signal assignment 
* `--` comment 

### Process

There can be multiple or none processes inside an architecture. A process groups commands under a process name.

`PROCESS` is the VHDL behavioral description keyword. Every VHDL behavioral description has to include a `PROCESS`. The statement `PROCESS (I1, I2)` , with `I1` and `I2` as inputs, is a concurrent statement, so its execution is determined by the occurrence of an event. `I1` and `I2` constitute a sensitivity list of the process. The process is executed \(activated\) only if an event occurs on any element of the sensitivity list; otherwise, the process remains inactive. If the `PROCESS` has no sensitivity list, the process is executed continuously.

### Data Flow Description 

* **Behavioral** describes _how_ the outputs change with the inputs to model the system.
  * usually used when the Boolean function or the digital logic is hard to obtain
* **Structural** models the system as components or gates or equations.
* **Switch-Level** is the lowest description model. It models the system with transistors and switches.

## Operators

<table>
  <thead>
    <tr>
      <th style="text-align:left">OPERATORS</th>
      <th style="text-align:left">OPERAND</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Logical</td>
      <td style="text-align:left"><code>AND</code>, <code>OR</code>, <code>NAND</code>, <code>NOR</code>, <code>XOR</code>, <code>XNOR</code>, <code>NOT</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Relational</td>
      <td style="text-align:left"><code>=</code>, <code>/=</code>, <code>&lt;</code>, <code>&lt;=</code>, <code>&gt;</code>, <code>&gt;=</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Arithmetic</td>
      <td style="text-align:left"><code>+</code>, <code>-</code>, <code>*</code>, <code>/</code>, <code>MOD</code>, <code>REM</code>, <code>ABS</code>, <code>&amp;</code>, <code>**</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Shift</td>
      <td style="text-align:left">
        <p>(logical shift) <code>SSL 1</code>, <code>SSL 2</code>, <code>SRL 1</code>, <code>SRL 2</code>
        </p>
        <p>(arithmetic shift) <code>SLA 1</code>, <code>SRA 1</code>
        </p>
        <p>(rotate) <code>ROL 1</code>, <code>ROR 1</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Signals

Signal-assignment statements are concurrent. At any simulation time, all signal-assignment statements that have an event are executed at the same time.

```cpp
SIGNAL a, b: std_logic;
```

You can also define your own type of signal as follows:

```cpp
TYPE StateType IS
		(Start_State, S_On1, S_On2, S_On3, S_On4, S_On5, S_On6, End_State);

SIGNAL CurrentState, NextState: StateType;
```

You can also **encode** \(use numbers instead of names\) for each state:

```cpp
SUBTYPE StateType IS std_logic_vector(3 DOWNTO 0);

SIGNAL CurrentState, NextState: StateType;
```

How do declare an array of vector signals:

```cpp
TYPE regArray_type IS ARRAY (0 TO 7) OF std_logic_vector(15 DOWNTO 0);
 
SIGNAL regArray : regArray_type := 
    (X"0000", X"000B", X"0023", X"0007", X"000A", X"0000", X"0000", X"0000"); 
```

### std\_logic\_vector

* All bits in a vector can be zeroed by using the aggregate assignment `(others => '0')`
* A signal is **high impedance** when all it's values are "Z"
  * no output

```cpp
random_signal_name_here <= (OTHERS=>'Z');
```

## Variables

Variable assignments are sequencial. 



## Events

* An event is a change in the value of a signal or variable \(e.g. from 0 to 1 or from 1 to 0\). 

```cpp
StateReg: PROCESS (Clk, Rst)
BEGIN
  IF (Clk = '1' AND Clk'EVENT) THEN
    IF (Rst = '1') THEN
      CurrState <= S_Off;
    ELSE
      CurrState <= NextState;
    END IF;
  END IF;
END PROCESS StateReg;
```

## Testbench

