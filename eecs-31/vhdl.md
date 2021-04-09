---
description: 'Nazeih Botros, "HDL with Digital Design VHDL AND VERILOG"'
---

# VHDL

Hardware Description Language \(**HDL**\) is a computer-aided design \(**CAD**\) used to design high-density programmable chips such as application-specific integrated circuits \(**ASICs**\) and field-programmable gate arrays \(**FPGAs**\), which are used in the design of digital systems. The two widely used hardware description languages are **VHDL** and **Verilog**.

### Structure of VHDL Module

The VHDL module consists of: `ENTITY` and `ARCHITECTURE`.

#### Entity

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
  * other types: `std_logic`, `real`, `integer` 
* semicolon indicates a new statement

{% hint style="warning" %}
VHDL is case insensitive!
{% endhint %}

#### Architecture

`ARCHITECTURE` describes the relationship between the inputs and the outputs of the system. 

{% hint style="danger" %}
`ARCHITECTURES` MUST be bounded to ONLY ONE entity, but multiple `ARCHITECTURES` can be bound to the same entity.
{% endhint %}

```cpp
ARCHITECTURE dtfl_half OF Half_adder IS
BEGIN
S <= a xor b; --statement 1
C <= a and b; --statement 2
--Blank lines are allowed
END dtfl_half;
```

The architecture recognizes the information declared in the entity.

* `<=` signal assignemnt 
* `--` comment 

### Data Flow Description 

* **Behavioral** describes _how_ the outputs change with the inputs to model the system.
  * usually used when the Boolean function or the digital logic is hard to obtain
* **Structural** models the system as components or gates.

