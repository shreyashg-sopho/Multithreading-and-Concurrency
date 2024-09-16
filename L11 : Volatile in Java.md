### Volatile in Java
The volatile keyword in Java is used to mark a variable as being stored in main memory. It ensures visibility and consistency of the variable's value across multiple threads. This keyword is essential in multi-threaded environments where different threads may access and modify shared variables.

### Use Case of volatile:
In Java, each thread typically has its own cache, and variables may be stored locally in the thread's cache for performance reasons. This can lead to a situation where:

One thread updates a variable in its cache.
Another thread doesn't see the updated value because it's still reading from its own cached version.
By marking a variable as volatile, you tell the JVM:

No Caching: The variable is not to be cached thread-locally. Every read and write to this variable will happen directly from and to the main memory.
Visibility: When one thread modifies a volatile variable, the change is visible to all other threads immediately.


#### Code : 
```java
public class ResolvingIssueUsingVolatile {
    private static volatile boolean isRunning = true; // Non-volatile flag

    public static void main(String[] args) throws InterruptedException {
        // Task is A loop that runs based on isRunning flag
        Runnable task = () -> {
            while (isRunning) {
                System.out.println("Value of is running in thread " + Thread.currentThread().getName() +" is " + isRunning);
            }
        };
        Thread threadA = new Thread(task, "A");
        Thread threadB = new Thread(task, "B");
        Thread threadC = new Thread(task, "C");
        Thread threadD = new Thread(task, "D");
        Thread threadE = new Thread(task, "E");
        Thread threadF = new Thread(task, "F");
        // Start child thread
        threadA.start();
        threadB.start();
        threadC.start();
        threadD.start();
        threadE.start();
        threadF.start();

        // Sleep for 10 millis
        Thread.sleep(10);


        isRunning = false; // But Thread B might not see this change!

        System.out.println("isRunning set to false.");
    }
}
```
#### Terminal Output :

```
Value of is running in thread A is true
Value of is running in thread A is true
Value of is running in thread A is true
Value of is running in thread A is true
Value of is running in thread C is true
Value of is running in thread E is true
Value of is running in thread C is true
Value of is running in thread B is true
Value of is running in thread B is true
Value of is running in thread F is true
Value of is running in thread D is true
Value of is running in thread F is true
Value of is running in thread B is true
Value of is running in thread F is true
Value of is running in thread F is true
.
.
.
.
.
isRunning set to false.
Value of is running in thread C is false
Value of is running in thread F is false
Value of is running in thread B is false
Value of is running in thread A is false
Value of is running in thread D is false
```

#### Illustrations
```
INITIALLY : 

           -----------------------         -----------------------         -----------------------
           |     Thread A         |       |     Thread B          |        |     Main Thread       |
           |                      |       |                       |        |                       |
           | CACHE (nothing)      |       |   CACHE (nothing)     |        |   CACHE (nothing)     |
           ------------------------        ------------------------        ------------------------

                       ^                                ^                                 ^
                       |                                |                                 |
                       |                                |                                 |
                       |                      -----------------------                     |
                        -------------------- |    Main Memory       |---------------------
                                             |    int flag = true   |
                                             ------------------------

VALUE UPDATED BY MAIN THREAD : 


           -----------------------         -----------------------         -----------------------
           |     Thread A         |       |     Thread B          |        |     Main Thread       |
           |                      |       |                       |        |                       |
           | CACHE (nothing)      |       |   CACHE (nothing)     |        |   CACHE (nothing)     |
           ------------------------        ------------------------        ------------------------

                       ^                                ^                                 ^
                       |                                |                                 |
                       |                                |                                 |
                       |                      -----------------------                     |
                        -------------------- |    Main Memory       |---------------------
                                             |    int flag = false  |
                                             ------------------------

(Everyone each time now is reading value from the Main Memory (RAM) itself).
```


### Applications of volatile:
#### 1.Flags and States: 
It is commonly used for variables that act as flags or states between threads. For example, a boolean flag to signal shutdown. Example seen above.

#### 2.Double-Checked Locking:
Volatile is used in patterns like the Double-Checked Locking Singleton to ensure that changes to the instance variable are visible across all threads.
```java

public class Singleton {
    private static volatile Singleton instance;

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

```

### Limitations of volatile:
#### No Atomicity: 
Volatile does not guarantee atomicity. For instance, if multiple threads are incrementing a volatile variable, a race condition may still occur because volatile only ensures visibility, not atomicity.
#### Only for Simple Operations:
 It is not a replacement for synchronization in more complex operations involving multiple variables or more complex logic.


### Conclusion :

In summary, the volatile keyword ensures the visibility of changes across threads and is useful in scenarios where you need a lightweight synchronization mechanism for shared variables. However, for more complex synchronization requirements (like atomic operations or ensuring consistency across multiple variables), you need to use other mechanisms like synchronized blocks or locks.

### Differences in Synchronization and Volatile :
- Synchronization only ensures that that the peice of code is being executed by only a single thread at once. It could still be reading values from cache. 
- Volatile only ensures that the value is beig read form main memory always, race conditions will still occur. volatile strictly and strictly only imporves on the `visibility` part of a variable, nothing else.







