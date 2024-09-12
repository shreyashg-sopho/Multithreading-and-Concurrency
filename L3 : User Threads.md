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
### Practival Example :
In the above program a user thread is spawned by the main thread.

A user thread can be used to handle a client request in a web server, where the thread's execution is critical to the application's functionality and needs to complete before the program terminates.


### 2. Daemon Threads

**Definition**: These threads run in the background and are typically used for performing supporting tasks like garbage collection.

### Characteristics:
- Terminate when no user threads are running. (If the main thread or all user threads are dead, all daemon threads will be killed mid way!)
- Can be created by calling the `setDaemon(true)` method on a thread.

### Example:

```java
Thread daemonThread = new Thread(() -> {
    while (true) {
        System.out.println("Daemon Thread Running");
    }
});
daemonThread.setDaemon(true);
daemonThread.start();
```

In the above program a user thread is spawned by the main thread.

### Practival Example :
A daemon thread can be used to perform periodic tasks like garbage collection or background maintenance in an application.



## Daemon Threads vs User Threads :

### Experiment 1 :
####  (Using User threads)
```java
public class TimePass2 {

    public static void main(String[] args) {
        // Create and start a user thread
        Thread userThread = new Thread(() -> {
            while (true) {
                System.out.println("UserThread is running...");
                try { Thread.sleep(500); } catch (InterruptedException e) { }
            }
        });
        userThread.setName("UserThread");
        userThread.start();

        // Main thread work
        try {
            Thread.sleep(2000); // Let the user thread run for a short while
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread is ending.");
        // The JVM will wait for the UserThread to complete before exiting.
    }
}

```

#### output : 
```
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
Main thread is ending.
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
.
.
.
.
Process finished with exit code 130 (interrupted by signal 2: SIGINT)

```


### Experiment 2 :
####  (Using Daemon threads)

```java
public class DaemonThreadDemo {

    public static void main(String[] args) {
        // Create and start a daemon thread
        Thread daemonThread = new Thread(() -> {
            while (true) {
                System.out.println("DaemonThread is running...");
                try { Thread.sleep(500); } catch (InterruptedException e) { }
            }
        });
        daemonThread.setName("DaemonThread");
        daemonThread.setDaemon(true); // Set this thread as daemon
        daemonThread.start();

        // Main thread work
        try {
            Thread.sleep(2000); // Let the daemon thread run for a short while
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread is ending.");
        // The JVM will terminate, and the DaemonThread will be stopped.
    }
}
```
#### output : 
```
UserThread is running...
UserThread is running...
UserThread is running...
UserThread is running...
Main thread is ending.
```

### Kya Samjhe ?
Yahi ki Daemon thread kisi aur User ya Main thread ke bharose hi hota hai. Agar koi dusra User ya Main thred exist nahi karega toh ye bhi kill ho jayega. 



