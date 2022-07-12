# Processes and Threads



## Process

A process is an instance of a program in execution. Every process needs some resources. A process address space contains the stack, heap, data and code.

Status:

1. New: process being created&#x20;
2. Ready: in main memory ready to be assigned to a processor
3. Running: CPU executing process
4. Waiting: for an event to happen&#x20;
5. &#x20;terminated: process finished execution

The kernel has a PCB (Process Control Block) for every process. The PCB stores information about the process state, counter, registers, scheduling information, memory management, accounting information, and IO statutes.

### Context Switch&#x20;

When the CPU switches from one process to another. When this happens the CPU must save the state of the old process into its PCB and load the state of the new process. Context switching has time overhead because the system does not do anything useful while switching.

### Process Scheduling&#x20;

A scheduler select among available processes for execution on CPU. The scheduler maintains three queues:

1. job queue: all processes in the system&#x20;
2. ready queue: processes in memory ready to be executed
3. device queues: processes waiting for IO device

There are two types of schedulers:&#x20;

1. short-term: selects which process to be executed next
2. long-term: selects which processes to be brought into the ready queue

## Threads

Is the basic unit of CPU execution
