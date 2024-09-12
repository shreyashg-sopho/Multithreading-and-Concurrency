# Context Switching in Operating Systems

## What is Context Switching?

**Context switching** is the process in which the **CPU switches from one thread (or process) to another**. During this switch, the state of the currently running thread (or process) is saved so that it can resume execution later, and the state of the next thread (or process) is loaded. This allows a single CPU core to manage multiple threads by sharing time among them.

## Example Scenario:

Imagine you have two OS threads:
1. **OS Thread 1** running a **music player**.
2. **OS Thread 2** running **MS Word**.

### How Context Switching Works:

1. **Thread 1 Execution**:
   - The CPU starts executing OS Thread 1 (music player).
   - During this time, the CPU has all the necessary data, registers, and program counter set for Thread 1.

2. **Saving State of Thread 1**:
   - After a short time slice (controlled by the operating system), the CPU **saves the current state** of Thread 1, including the values of its registers, program counter, and other necessary information, in memory (usually in the thread control block or process control block).
   - This is done so that the CPU can resume execution of Thread 1 exactly where it left off.

3. **Thread 2 Execution**:
   - The CPU then **loads the state of Thread 2** (MS Word), which was saved when Thread 2 was last paused.
   - The CPU switches its focus to Thread 2 and starts executing its instructions.

4. **Switching Back**:
   - After another time slice, the CPU **saves the state** of Thread 2 and **loads the state** of Thread 1 again, continuing from where it left off.
   - This process repeats, alternating between the two threads as needed.

## Diagram of Context Switching (Simplified)
<img width="707" alt="Screenshot 2024-09-12 at 10 42 03 PM" src="https://github.com/user-attachments/assets/3aad7bab-c960-40e6-b91a-ba9b985aad53">


## Key Concepts

- **Saving and Loading State**: Context switching involves saving the entire state of a thread (or process), including registers, program counter, and other processor-specific information, and then loading the state of another thread to resume its execution.
- **Time-Slicing**: The operating system typically uses a scheduling algorithm (e.g., round-robin) to divide CPU time between threads or processes.
- **Overhead**: Context switching comes with overhead, as it takes time to save and restore the states of threads, but it enables multitasking and allows a single core to handle multiple tasks.

## Summary

Context switching enables multitasking by alternating between threads, saving and restoring their states so they can resume execution later. While it introduces some overhead, it is essential for enabling a single CPU core to manage multiple threads or processes seemingly simultaneously.
