
### States of a Thread :
- NEW
- RUNNABLE
- WAITING
- TIMED_WAITING
- BLOCKED
- TERMINATED
(This can also be found in Thread.java class)




#### Note : 
"Running" is not actually a state as referred throughout this article, it is only mentioned to represent that the thread is currently geeting executed in CPU.



### Threads ka state machine :
```
                +-----------------+
                |      NEW        |
                +-----------------+
                        |
                      start()
                        |
                        v 
                 +-----------------+                                                                        
                 |     Runnable    | <----------------------------------------------------------------------                                                          
                 +-----------------+                       |   (when waiting finish )                      |
                   ^            |                +-----------------+                                       |
                   |            |                |    Waiting      |                                       |
     Preempted or  |            |                |or timed waiting |                                       |
     thread.yielf()|            |                +-----------------+                                       |
    or time sliced |            |(SCHEDULED by CPU)       |                                                |
                   |            |                         |                                                |
                   |            |                         |                                                |
                   |            v                         |                                                |
                +-----------------+-----------------------       +-----------------+                       |
                |     Running     | -------------------------->  |    Terminated   |                       |                                      
                +-----------------+       run() terminates       +-----------------+                       |                                                          
                        |                                                                                  |
                   seeking lock (synchronized method called pr reenterant lock  or so                      |
                        |                                                                                  |                                                        
                        |                                                                                  |
                        -----------------------if not obtained lock ------->  +-----------------+-----------
                                                                              |     Blocked     |
                                                                              +-----------------+

```            
## Thread States and Transitions :

### Thread States and Transitions Table :

| **State**             | **Description**                                                                                                                                              | **Transition From This State**                                                                                                                       |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NEW**               | A thread is created but has not yet started execution.                                                                                                        | - `start()` method is called, and the thread moves to the **RUNNABLE** state.                                                                         |
| **RUNNABLE**          | The thread is ready to run and is waiting to be scheduled for execution by the CPU. It may or may not be running at any point in time.                        | - The CPU schedules the thread, and it moves to **RUNNING**.<br> - If preempted (via `Thread.yield()`), time-sliced, or blocked, it moves back here. |
| **RUNNING**           | The thread is actively executing its task.                                                                                                                    | - If the thread finishes execution (`run()` method completes), it moves to **TERMINATED**.<br> - If it seeks a lock and obtains it, it continues in **RUNNING**. If it cannot obtain the lock, it moves to **BLOCKED**.<br> - If the thread calls `wait()`, it transitions to **WAITING**.<br> - If the thread calls `wait(n)` or `join(n)`, it transitions to **TIMED_WAITING**.<br> - If preempted or the time slice expires, it returns to **RUNNABLE**. |
| **BLOCKED**           | The thread is trying to acquire a lock but another thread holds the lock. The thread cannot proceed until the lock is released.                               | - Once the lock is obtained, the thread returns to **RUNNABLE**.                                                                                      |
| **WAITING**           | The thread is waiting indefinitely for another thread to perform a particular action, such as `notify()` or `notifyAll()`.                                    | - If notified by another thread (`notify()`/`notifyAll()`), it returns to **RUNNABLE**.<br> - It can also be interrupted and return to **RUNNABLE**.  |
| **TIMED_WAITING**     | The thread is waiting for a specified period, such as using `wait(n)` or `join(n)`.                                                                           | - When the specified time expires, the thread returns to **RUNNABLE**.<br> - If interrupted or notified earlier, it also returns to **RUNNABLE**.    |
| **TERMINATED**        | The thread has completed execution.                                                                                                                           | - No further transitions; the thread’s lifecycle is complete.                                                                                        |
 
### Thread Transitions Table :

| **From State**        | **To State**        | **Condition / Action**                                                                                                                                                       |
|-----------------------|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NEW**               | **RUNNABLE**        | The thread is started by calling the `start()` method.                                                                                                                        |
| **RUNNABLE**          | **RUNNING**         | The CPU schedules the thread for execution.                                                                                                                                   |
| **RUNNING**           | **BLOCKED**         | The thread attempts to enter a synchronized block or method and fails to obtain the lock.                                                                                      |
| **RUNNING**           | **WAITING**         | The thread calls `wait()` or `join()` and waits indefinitely for another thread’s action.                                                                                      |
| **RUNNING**           | **TIMED_WAITING**   | The thread calls `wait(n)` or `join(n)` and waits for a specified time period.                                                                                                 |
| **RUNNING**           | **RUNNABLE**        | The thread is preempted by the CPU (e.g., `Thread.yield()` is called), or the time slice expires.                                                                              |
| **RUNNING**           | **BLOCKED**         | The thread attempts to acquire a lock and is blocked because the lock is held by another thread.                                                                               |
| **RUNNING**           | **TERMINATED**      | The `run()` method completes, and the thread’s task finishes or an exception is thrown                                                                                         |
| **BLOCKED**           | **RUNNABLE**        | The thread acquires the lock it was waiting for.                                                                                                                               |
| **WAITING**           | **RUNNABLE**        | Another thread calls `notify()`, `notifyAll()`, or the thread is interrupted.                                                                                                  |
| **TIMED_WAITING**     | **RUNNABLE**        | The specified wait time expires, or the thread is notified or interrupted by another thread.                                                                                    |


### Related articles :
https://herovired.com/learning-hub/blogs/life-cycle-of-thread-in-java/#

