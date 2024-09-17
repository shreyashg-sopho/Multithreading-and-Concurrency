### Problem Statement :
When you have a system where one or more threads are producing tasks (or data) and one or more threads are consuming those tasks, coordination between the threads can be challenging. Producers shouldn't overload consumers, and consumers should wait until there's something to consume.


###  Let's create Consumer Producer Problem : 


#### Simple Queue impl
```java
import java.util.*;
public class Solution {
  public static class MyQueue<T> {
      private List<T> queue = new ArrayList<>();
      private int maxSize;
      public  MyQueue(int size)
      {
       this.maxSize = size;
      }

      public void remove()
      {
          if (queue.size() > 0)
          {
              System.out.println(System.currentTimeMillis() + " " + Thread.currentThread().getName() + " : " + "Popping an element out " + queue.remove(0));
          }
          else
          {
              System.out.println(System.currentTimeMillis() + " " + Thread.currentThread().getName() + " : " + "Nothing to pop out yet");
          }
      }

      public void add(T val)
      {
          if(queue.size() < maxSize )
          {
              System.out.println(System.currentTimeMillis() + " " + Thread.currentThread().getName() + " : " + "Adding to the queue " + val);
              queue.add(val);
          }
          else
          {
              System.out.println(System.currentTimeMillis() + " " + Thread.currentThread().getName() + " : " + "No spapce left in queue");
          }
      }

  }


    public static void main(String[] args) {
        MyQueue<Integer> myQueue = new MyQueue<Integer>(3);
        Thread t0 = new Thread(() -> {
            myQueue.add(1);
            myQueue.add(2);
            myQueue.add(3);
            myQueue.add(4);
            myQueue.add(5);
        } );


        Thread t1 = new Thread(() -> {
            myQueue.remove();
            myQueue.remove();

        } );

        t0.start();
        t1.start();

    }
}

```
#### Terminal Output : 
```
1726572009206 Thread-0 : Adding to the queue 1
1726572009206 Thread-1 : Nothing to pop out yet
1726572009206 Thread-0 : Adding to the queue 2
1726572009206 Thread-1 : Popping an element out 1
1726572009206 Thread-0 : Adding to the queue 3
1726572009207 Thread-0 : Adding to the queue 4
1726572009207 Thread-0 : No spapce left in queue
```


#### Explantion :
This code works well in a single-threaded or non-concurrent environment where one operation follows another. If an element can't be added because the queue is full, it simply moves to the next operation, and if there’s nothing to remove, it prints a message and moves on.

However, in a multi-threaded environment (like the producer-consumer problem), this approach runs into several challenges:

- **Race Conditions**: Multiple threads can access and modify the queue simultaneously, which can lead to unpredictable behavior. For example, two threads might try to add or remove elements at the same time, causing inconsistent or incorrect results.
- **Producer Overload**: If the queue is full, producers should wait for space instead of it trying to  add more tasks when there is no space to accomodate.
- **Consumer Waiting**: If the queue is empty, consumers should wait for new tasks instead of trying to remove when there is nothing to remove.

#### Solution
To handle these issues, you would typically use a `Blocking Queue`, which ensures that:

Producers wait when the queue is full, until consumers make space by removing elements.
Consumers wait when the queue is empty, until producers add elements to the queue.


This ensures proper coordination between the producer and consumer threads, preventing race conditions and allowing the system to run smoothly in a multi-threaded environment.

