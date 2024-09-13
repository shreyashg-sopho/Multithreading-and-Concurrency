

# run() vs start() Method in Java Thread

In Java, the run() and start() methods are both used in the context of threading, but they serve very different purposes. Here's an explanation of their differences:

## 1. run() Method

- The run() method contains the code that is executed by the thread. 
- **If you call the `run()` method directly**, it will not create a new thread; instead, it will run the code in the current thread, just like a normal method call.

### Example:
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running in: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.run();  // This does not create a new thread.
        System.out.println("Running in: " + Thread.currentThread().getName());
    }
}
```

## 2. start() Method

- The start() method is responsible for actually creating a new thread and then calling the run() method within that new thread.
- When you call the start() method, a new thread is created, and the run() method is invoked on that thread.


### Example:


```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running in: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();  // This creates a new thread and calls run() in it.
        System.out.println("Running in: " + Thread.currentThread().getName());
    }
}

```


## Conclusion


- Call run() directly if you do not want to create a new thread (but this defeats the purpose of multithreading).
- Call start() to create a new thread and execute the code in the run() method concurrently.



