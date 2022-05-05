---
description: Introduction, Computer System Structures, and Operating System Structures
---

# Operating Systems

## Computer System

A computer system can be divided into four components:&#x20;

1. **Hardware:**
   * provides resources for computing&#x20;
   * ex: CPU, memory, IO devices
2. **Operating System:**
   * software that acts as an intermediary between the user applications in your computer and the computer hardware&#x20;
3. **Application Programs:**
   * solves computing problems for the users using the computer resources
   * ex: word processors, compiler, text editor
4. **Users:**
   * people, other computers

### Computer System Organization&#x20;

When you start your computer, your computer runs the **bootstrap program** which will load the operating system from memory and start executing the kernel.&#x20;

All hardware has controllers or drivers and they are all connected through a common bus to the memory (like RAM). The hardware and the controllers are IO devices. The drivers are responsible for moving data between the devices that it controls to the local buffer storage.

The CPU, which is not an IO device is also connected to the memory through the common bus. The CPU can only load instructions from memory.

![](<../.gitbook/assets/image (14).png>)

### Interrupts&#x20;

Interrupts execution from the CPU to a different location. The occurrence of a (most of the time IO) event is signaled by an interrupt to the CPU. Interrupts can be generated from hardware or software (we call these "traps" and they can come from exceptions or whatnot).

Type of interrupts:

1. **Polling**:&#x20;
   1. same interrupt handler for all interrupts
   2. set flags that must be periodically checked by the CPU
   3. the overhead of polling depends on the polling frequency
2. **Interrupt Vector Table**:&#x20;
   1. signal to the CPU indicates the type of interrupt and the execution control will be transferred to a specific interrupt handler.

An interrupt handler must preserve the state of the CPU by storing the interrupted instruction address in one of the registers so that it can be loaded later.&#x20;

A special timer interrupt is also started to protect the CPU from instructions that may execute indefinitely. When the timer runs, an interrupt is issued and the execution returns to the CPU.

{% hint style="info" %}
Raising interrupts is privileged
{% endhint %}

### Process Abstraction

A **process** is an instance of a program, running with limited rights. A **thread** is a sequence of instructions running within a process. Potentially, there can be many threads in a process.

### Memory Protection

It is important to isolate memory in order to protect it from users. We do so by virtual addresses that the processor maps to physical memory. To implement the protection, two registers will hold the addresses range of valid memory that a program may access: base register and a limit register.&#x20;

{% hint style="info" %}
The load instructions for base and limit are **privileged**
{% endhint %}

### IO Devices

Users program can't access IO devices because they are privileged. The user uses **system calls** in order to control them. Systems calls are used by users to request an action to the hardware.

### Storage Structure

There are two types of storage: **volatile**, which gets wiped out when the computer doesnt get power, and **non-volatile**, which doesnt. The CPU has direct access to the main memory. Instructions are stored in the main memory. Main memory is an array of addressable words or bytes. All secondary storages (below the main memory) are non-volatile and they are used for backup because main memory and registers are expensive.

Caching copies information into faster storage systems.&#x20;



## Operating System

Operating System, usually called the kernel, is the intermediary between users and hardware.&#x20;

### OS roles are:

1. **Referee**
   * resource allocation&#x20;
   * isolation of data
     * prevents data corrupted&#x20;
     * prevents security issues&#x20;
   * communication between users and applications when necessary
2. **Illusionist**&#x20;
   * each application appears to have the entire machine to itself
     * applications operate under the assumption that they have unlimited storage, memory, processor, reliable network connections
     * OS hides the management of all these resources&#x20;
3. **Glue**
   * reduces the cost of developing software through abstraction

### OS Challenges:

1. Reliability
2. Availability&#x20;
3. Security&#x20;
4. Privacy
5. Performance
   1. latency or response time
   2. throughput
   3. overhead
   4. fairness&#x20;
   5. predictability&#x20;
6. Portability
   1. hardware abstraction level&#x20;
   2. APIs

## Dual Mode Operation

Hardware provides protection for CPU through two modes of operation:

1. **User mode**
   1. executing privileged instructions will "trap" it and returns the execution to the kernel mode
2. **Kernel Mode**
   1. executions made on behalf of the system
      1. programming the kernel
   2. can execute **privileged instructions**
   3. unrestricted access to all memory

## Monolithic vs Microkernel OS

1. Monolithic&#x20;
   1. Deals with more components
   2. More complex
2. Microkernel&#x20;
   1. Moves as much as possible stuff from the kernel space to the user space
   2. Simpler, more reliable and secure, and it has more portability
   3. There is performance overhead for naive implementation

