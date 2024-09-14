## What is synchronised keyword ?

If multiple threads are trying to operate simultaneously on the same java object then there may be a chance of data inconsistency problem. To overcome this problem we should go for the `synchronized` keyword, then at the time only one thread is allowed to execute that method or block on the given object so that data inconsistency problem will be resolved.

The main advantage of the `synchronized` keyword is we can resolve the data inconsistency problem but the main disadvantage of the `synchronized` keyword is, it increases waiting time thread and creates performance problem, hence it is not recommended to use synchronized keyword.

If a thread wants to execute the synchronized method in the given object:
-  it has to get a lock of that object
-  once thread got the lock then it is allowed to execute any synchronized method on that object.
-  once method execution completes automatically thread releases lock.
when a thread executing a synchronized method on the given object the remaining thread is not allowed to execute simultaneously on the same object but the remaining thread is allowed to execute a non-synchronized method simultaneously.



### Code block 1 
```java
Integer hours = 20;
private void sleeping ()
{
   System.out.println("I am sleeping and resting for " + hours + "hrs));
}
```

### Code block 2 
```java
Integer hours = 20;
private void sleeping ()
{
   synchronized(hours) {
      System.out.println("I am sleeping and resting for " + hours + "hrs));
   }
}
```
