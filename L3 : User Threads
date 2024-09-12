### 1. User Threads

**Definition**: These are the threads that the application developer explicitly creates to perform specific tasks.

**Characteristics**:
- Keep the program running as long as they are alive. (Even if the main thread dies, but if any user thread exists, the program will keep running until all user threads die).
- Created by extending the `Thread` class or implementing the `Runnable` interface.


**Example**:

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("User Thread Running");
    }
}

MyThread thread = new MyThread();
thread.start();
```

In the above program a user thread is spawned by the main thread.




