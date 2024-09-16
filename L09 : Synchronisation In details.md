

 
### How Synchronization Works?
We saw why schronization is needed ? Now let's proceed to how can we use it in Java.

Synchronization in Java is provided in two ways:



#### 1. Synchronized Method
In synchronized methods, the current object (this) is used as the lock. If a static method is synchronized, the class object (ClassName.class) is used as the lock.
``` java
class Counter {
    private int count = 0;

    // Lock Object: 'this' (the current instance)
    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```
In this example, the entire method is synchronized using the current instance (this) as the lock object.


#### 2. Synchronized Block
In a synchronized block, you can explicitly specify any object as the lock. This allows more control over the synchronization scope and performance.
```java
class Counter {
    private int count = 0;
    private final Object lock = new Object();

    // Lock Object: 'lock' (custom lock object)
    public void increment() {
        synchronized(lock) {
            count++;
        }
    }

    public int getCount() {
        return count;
    }
}
```
Here, we are using a custom lock object to synchronize only a portion of the code instead of the entire method.


#### 3. Static Synchronization (Class-level Lock)
When synchronizing static methods, the class object (ClassName.class) is used as the lock.

```java
class StaticCounter {
    private static int count = 0;

    // Lock Object: Static class (StaticCounter.class)
    public static synchronized void increment() {
        count++;
    }

    public static int getCount() {
        return count;
    }
}
```
In this case, the lock object is the StaticCounter.class, meaning only one thread can execute the synchronized static method across all instances.



#### 4. Synchronization on Arbitrary Objects
You can use any object as a lock, allowing finer control over which resources are synchronized.
```java
class CustomLockExample {
    private final Object lock = new Object();
    private int sharedResource = 0;

    // Lock Object: 'lock' (custom lock object)
    public void accessResource() {
        synchronized(lock) {
            sharedResource++;
        }
    }
}
```

In this example, we synchronize on a custom lock object rather than this, ensuring that other parts of the class can run concurrently.




