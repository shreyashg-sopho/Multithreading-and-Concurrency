Let's try to implement a simple counter.

### Approach 1  : A simple counter
#### Code 1
```java
public class SomeClass {
    public static  int counter = 0;

    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(new CountingTask());
        Thread t2 = new Thread(new CountingTask());
        Thread t3 = new Thread(new CountingTask());
        t1.start();
        t2.start();
        t3.start();
        Thread.sleep(10);
        System.out.println("Value of counter is :: " + counter);
    }

    public static class CountingTask implements Runnable
    {

        @Override
        public void run() {
            for(int i = 0 ; i <  10000; i++)
            {
                counter++;
            }
        }
    }
}
```

#### Terminal output
```
Value of counter is :: 15441
```

#### Conmments :
The outcome ishould have been 30000, but it turned out to be only 15441. This is because in a a multi-threaded environment where the variable (`counter` in this case) is getting shared, is being read and write by many threads, which is leading to high inconsistency for reading the correct value of the variable `counter1`, leading to wring values being read by one. This is not a thread safe code.


### Approach 2  : Using volatile int for counting
#### Code 2

```java
public class SomeClass {
    public static volatile int counter = 0;

    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(new CountingTask());
        Thread t2 = new Thread(new CountingTask());
        Thread t3 = new Thread(new CountingTask());
        t1.start();
        t2.start();
        t3.start();
        Thread.sleep(10);
        System.out.println("Value of counter is :: " + counter);
    }

    public static class CountingTask implements Runnable
    {

        @Override
        public void run() {
            for(int i = 0 ; i <  10000; i++)
            {
                counter++;
            }
        }
    }
}
```



#### Terminal output
```
Value of counter is :: 21150
```

#### Conmments :
Again the expectation was actually 30000, but the count is stuck at 21150. Even though we have gotten rid of inconsistent reads, but race conditions are still happening while operating on the variable `counter`.

In this example, race conditions still occur while incrementing the counter. For instance, if Thread T1 and Thread T2 both read the value of counter as 12, they may both increment their local copies and write back the same value (13), thus losing updates made by each other.

### Approach 3  : Using Synchronization int for counting
#### Code 3
```java

public class SomeClass {
    public static int counter = 0;
    public static Object lock = new Object();
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(new CountingTask());
        Thread t2 = new Thread(new CountingTask());
        Thread t3 = new Thread(new CountingTask());
        t1.start();
        t2.start();
        t3.start();
        Thread.sleep(10);
        System.out.println("Value of counter is :: " + counter);
    }

    public static class CountingTask implements Runnable
    {

        @Override
        public void run() {
            for(int i = 0 ; i <  10000; i++)
            {
                synchronized (lock)
                {
                    counter++;
                }
            }
        }
    }
}
```


#### Terminal output
```
Value of counter is :: 30000
```
#### Conmments :
Using counter in synchronized block now ensure that only one thread is able to access the coutner at once, thus ensuring that the operation will be atomic and no issues in updating the values at once.

The only drawback is that this is more expensive and there exists a cheaper solution than this (time wise). This is expensive beacuse :
- Involves taking locks.
- Also the over head of managing synchronized code block which increass complexity.

### A Better Approach: Using AtomicInteger

To ensure both atomicity and visibility, we should use AtomicInteger from the java.util.concurrent.atomic package. AtomicInteger provides thread-safe operations without the need for explicit synchronization.

### How does it do that ?
AtomicInteger is optimized for performance with concurrent operations. It uses low-level atomic operations provided by the hardware (such as compare-and-swap), which generally have less overhead compared to locking mechanisms.


### Approach 4 :  Counter using Atomic Integer :
#### Code 4 :
```java
import java.util.concurrent.atomic.AtomicInteger;

public class SomeClass {
    public static AtomicInteger counter = new AtomicInteger(0);
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(new CountingTask());
        Thread t2 = new Thread(new CountingTask());
        Thread t3 = new Thread(new CountingTask());
        t1.start();
        t2.start();
        t3.start();
        Thread.sleep(10);
        System.out.println("Value of counter is :: " + counter);
    }

    public static class CountingTask implements Runnable
    {

        @Override
        public void run() {
            for(int i = 0 ; i <  10000; i++)
            {
               counter.getAndIncrement();
            }
        }
    }
}
```

#### Terminal output
```
Value of counter is :: 30000
```

#### Comments : 
Now that we are using atomicIntger, we do not need explicit locking or synchronization. Atomic Integer internally uses a volatile int to manage the visibility part. And
and efficient atomic operations to manage updates. This approach combines correctness with performance and simplicity.



### Conclusion :
- **Approach 1** : demonstrates the problem of race conditions without synchronization. (Not a thread safe code)
- **Approach 2** : addresses visibility issues but fails to prevent race conditions.
- **Approach 3** : ensures correctness through synchronization but introduces overhead and complexity. (A thread safe code but overkill)
- **Approach 4** : provides an efficient, thread-safe solution using AtomicInteger, combining correctness with performance and simplicity.


For simple counters and similar operations, AtomicInteger is often the best choice due to its efficiency and ease of use. For more complex scenarios involving multiple shared resources or operations, explicit `synchronization` might still be necessary.




# Important Note :
Know that Integer and AtomicInteger are not related. They both belong to separate packages and one is not the sub set of other. 
Atomic Package also provides other implementations  :
https://github.com/JetBrains/jdk8u_jdk/tree/master/src/share/classes/java/util/concurrent/atomic


















