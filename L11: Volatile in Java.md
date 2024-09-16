### Volatile in Java
The volatile keyword in Java is used to mark a variable as being stored in main memory. It ensures visibility and consistency of the variable's value across multiple threads. This keyword is essential in multi-threaded environments where different threads may access and modify shared variables.

### Use Case of volatile:
In Java, each thread typically has its own cache, and variables may be stored locally in the thread's cache for performance reasons. This can lead to a situation where:

One thread updates a variable in its cache.
Another thread doesn't see the updated value because it's still reading from its own cached version.
By marking a variable as volatile, you tell the JVM:

No Caching: The variable is not to be cached thread-locally. Every read and write to this variable will happen directly from and to the main memory.
Visibility: When one thread modifies a volatile variable, the change is visible to all other threads immediately.

### Applications of volatile:
#### 1.Flags and States: 
It is commonly used for variables that act as flags or states between threads. For example, a boolean flag to signal shutdown:


##### Code : 
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


This time there willbe no middle caching layer to read this value from. Value will directly be read from and updated in RAM 





