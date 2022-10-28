---
description: >-
  MIPS (Microprocessor without Interlocked Pipelined Stages) is a family of
  reduced instruction set computer (RISC) instruction set architectures (ISA)
---

# MIPS Processor

All MIPS instructions are 32-bits long and there are three types of instruction:&#x20;

* R-type: arithmetic&#x20;
* I-type: immediate
* J-type: jump

## Branches vs Jumps

These instructions are used when you do not want to execute the next PC value and want to transfer control to another part of the instruction space.

| branch                                                               |                                              |
| -------------------------------------------------------------------- | -------------------------------------------- |
| conditional transfer of control                                      | unconditional transfer of control            |
| target address is close to the current PC location                   | far away                                     |
| example: loops, if statements                                        | example: subrutine calls                     |
| target address is relative to the current branch instruction address | target address uses an absolute word address |

* branch&#x20;
* jump: concatenate the top4 bits of the pc, the 26 bit jump address and 00 to account for address offset
  * `pc = {pc_plus4[31:28], instruc[25:0] << 2}`&#x20;

## Data Memory&#x20;

* write: controlled synchronously&#x20;
  * inside an always block with the posedge of the clock
    * `always @(posedge clk) begin ...`
* read: asynchronous&#x20;
  * this means that at any moment, whenever read\_en is driven high, and there is a valid memory address, then the data is read
  * uses the `assign` statement outside the always clock

