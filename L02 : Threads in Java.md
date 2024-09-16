# Threads in Java

A [Thread](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html) is an independent sequential path of execution within a program. Multiple threads can run concurrently within the same program. At runtime, threads in a program share a **common memory space** and can therefore share both data and code.

### Three Important Concepts Related to Multithreading in Java:

1. **Creating Threads and Executing Code**:  
   Threads can be created by implementing the `Runnable` interface or extending the `Thread` class. The code to be executed by a thread is provided in the `run()` method.

2. **Synchronization**:  
   Threads can access common data and code, but this can lead to race conditions if not handled properly. Synchronization ensures that only one thread accesses critical sections of code at a time, preventing data corruption.

3. **Thread States**:  
   A thread can be in different states, such as `NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, or `TERMINATED`. The transition between these states occurs based on the thread's lifecycle and its interactions with the system or other threads.


