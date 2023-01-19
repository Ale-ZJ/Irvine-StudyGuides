---
description: synchronization and coordination among cooperating processes
---

# Process Synchronization

A **race condition** is when several processes access and manipulate the same data concurrently and the outcome of the execution depends on the particular order in which the access takes place, hence the output is random.

When a process executes its **critical section,** no other process is allowed to execute that critical section.

```
do{
   // enter critical section
      // critical section
   // exit critical section
   ... 
}while(true)
```

Assuming that each process finishes executing its critical section once entered, a solution to the critical-section problem must satisfy the following three requirements:

1. Mutual Exclusion: if process P\_i is executing its critical section when no other processes can be executing in their critical section
2. Progress: All threads will eventually run their critical section, they cannot be postponed indefinitely
3. Bounded waiting: there is a limit on the number of times other processes are allowed to enter their critical section.

## Mutex

A mutex has a boolean variable that indicates whether the lock is available or not. A process that wants to enter its critical section must `acquire()` a lock, and `release()` it when they exit. Mutex stands for (mutual exclusion)

This requires busy waiting until the process with the lock releases it, which might take a long time depending on application.

## Semaphore

A semaphore is an integer variable that is only accessed through two atomic operations: wait() and signal()
