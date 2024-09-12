## Multitasking : 
### Process based multitasking : 
Two different processesing running concurrently. Example : Youtube on browser playing music and Writing in sublime. Two different processes running concurrently.

### Thread based multitasking : 
A part of the same process running concurrently. Example : 
1. One thread in youtube playing music, while the other fetching data from youtube for recommendations.
2. One thread taking care of writing on Note pad, while the other thread taking care of suggesting spellings for auto correct.


## Context Switching
Context Switching is the process of CPU changing context from one thread/process to another.
Example One CPU core is running music player in one 1 CPU cycle and MS word in other cycle alternatingly. This is an example of context switching.

#### Context switching between two threads of same processes VS between two different processes  :
The threads of a same process lie in the same adress space hence context switching is less expensive within the same process vs two diff process. 

### Why Multithreading ?
Because in a single threaded environment only one task will be performed. CPU cycles could be wasted if the thread keeps waiting for a response from some external IO.
Where as in a multi threaded enviroment multiple tasks can be executed parallely or concurrently. CPU cycles are not wasted and instead can be utilized by other threads.
(If multiple cores then parallely, if single core then concurrently)

### Parallelism vs Concurrency 
Cores ka logic, CPU GPU mei yahi farak hai.
(Iska alag Repo banega)

## Threads 
(https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html)
Thread is an independent sequential path of execution within a program.
Many threads can run concurrently within a same program.
At runtime, threads in a program exist in a common memory space and can therfore share both data and code.

Three Important concepts related to Multithreading in Java : 
1. Creating threads and providing the code that gets executed by a thread.
2. Accessing common data and code thigh synchronization.
3. Transitioning between state of threads.




## Types of thread

























