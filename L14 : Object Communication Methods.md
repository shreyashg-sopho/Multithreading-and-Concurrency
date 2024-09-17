**Object class Link** : https://github.com/JetBrains/jdk8u_jdk/blob/master/src/share/classes/java/lang/Object.java

Each object in Java has these methods implemented : 
- wait (3 variants)
- notify()
- notifyAll()




### Wait : 
The wait() methods in Java are used for inter-thread communication. They allow a thread to release the lock it holds on an object and wait until another thread notifies it to wake up. These methods are used in conjunction with synchronization and are part of the java.lang.Object class. Here are the three variants of the wait() method:
#### 1. wait()
The thread that made this function call, it will release the lock object and will get the lock back once some other thread call's notify or notifyAll .
```java
synchronized (obj) {
    obj.wait(); // Wait until someone notifies.
}
```

#### 2. wait(timeout)
The thread that made this function call, it will release the lock object and will : 
- Get the lock back once some other thread call's notify or notifyAll .
- Or the timeout has passed and it will try to reacquire the lock.
```java
synchronized (obj) {
    obj.wait(1000); // Wait for 1 second
}
```
#### Note  : 
Re-acquiring the Lock: Once the timeout expires, the waiting thread is moved from the waiting state to the runnable state. The thread then attempts to re-acquire the lock on obj. This re-acquisition is not guaranteed to be immediate, as it depends on whether other threads are currently holding the lock or not.


#### 3. wait(timeoutInMillis, timeoutInNanos)
Same as above, just that the time is specified in to nanos as well. 
Ex : wait(1000, 1) , it means wait for a total of 1 sec and 1 nano second = 1000000001 nano seconds. (Faltu hai bhai, nahi need padega iska itna, ignore )
```java
synchronized (obj) {
    obj.wait(1000, 500000); // Wait for 1.5 seconds
}
```

### Notify 
The notify() and notifyAll() methods are used to wake up threads that are waiting on an object's monitor. These methods must be called from a synchronized context on the same object.

#### notify(): 
This method wakes up one of the threads that are waiting on the object's monitor. The choice of which thread to wake up is implementation-dependent. (usually random if every other thread is just using wait(), but can be controlled for threads differently by making threads give different timeout values and so on).
```java
synchronized (obj) {
    obj.notify();
}
```

#### notifyAll():
This method wakes up all the threads that are waiting on the object's monitor. 

```java
synchronized (obj) {
    obj.notifyAll();
}
```

### Note :
If we call any of these methods without actually taking any lock on the object, we will get an IllegalMonitorException or so. Because it means nonbody is taking lock on this obj, there is no need to use methods like wait and notify.





