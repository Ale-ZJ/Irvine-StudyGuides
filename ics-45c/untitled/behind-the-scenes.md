---
description: week 2 - understand the low level side of C++
---

# Behind the Scenes

#### How to understand the abstraction layer of your programming language

Other programming languages are considered higher abstraction level because there are fewer details that you have to handle.&#x20;

* What are you allowed to say?
* What are the semantics?
* What effects does the construct have on the performance?
* How to decompose a larger program into smaller pieces? How do reuse pieces?
* Who decides how and when values are organized in memory?
* How do you interact with devices other than the processor and memory?

The abstraction level of C++ abstraction level is close to the abstraction level of your machine (it is of lower level).

To use C++, we need to better understand some eecs stuff:

* Computer organization  (processors, registers, the memory hierarchy)
* The runtime organization of memory&#x20;
* What determines the real cost of the things we're doing

#### The Von Neuman Architecture

* **CPU**: grab one instruction at the time&#x20;
* **Main memory**: stores instructions and data&#x20;
* **bus**: wires that connect from the CPU to the main memory&#x20;
* **registers**: scratch paper inside CPU to temporarily remember small stuff&#x20;
  * one of them is called: **instruction pointer**&#x20;
    * tells you where in the main memory are stuff stored

1- Read instruction \
2- Decode it - what bits do you need - processes - what to do\
3- read operands\
4- execute the instruction \
5- store the result&#x20;

\--> every statement and expressions in code is a collection of instructions

#### Implementing C++ features

A C++ compiler has two jobs:  //basically it translates into assembly language\
(1) Check syntax and semantics \
(2) Emit machine instructions that have the same observable effect as your program\
&#x20;         \-- includes optimizations

#### Implementing variables

When you are declaring a variable, you are asking for memory space.&#x20;

#### Implementing control structures&#x20;

CPUs have "jump instructions" (the next thing you need to do is over there) and "branch instructions" (there is a condition)

```cpp
if (???) <--- conditional expression of an integral type
```

If the conditional expression evaluates to 0 then the expression is False, any other value is True (0=FALSE, anything else = TRUE). The C++ if statement is the same.

#### What about functions?

There is a cost for the organization. Run time stacks have activation records that keep track of all the variables and stuff needed to run a function.

