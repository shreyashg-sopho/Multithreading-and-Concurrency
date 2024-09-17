### Approaching solution to the L13 :  Consumer Producer Problem
We can solve the problem mentioned in consumer problem if we are able to :
- If there is nothing to remove, let's wait until something is added. 
- If there is no space to add, let's wait until something is removed.
But ensure that while doing all of the above, let's not block the resource either otherwise that will result in a deadlock. 


### Solution 

We can achieve this by :
```
def remove ():
    acquire lock on queue

    while queue is empty:
        wait for notification (release lock during wait)

    item = remove item from queue
    print "Removed: " + item
    notify others (release the lock and let someone else acquire and do there check or whatever)



def add (T val)
      acquire lock on queue


      while queue is full:
          wait for notification (release lock during wait)


      queue.add(val)
      notify others (release the lock and let someone else acquire and do there check or whatever)


```

#### Implementing our own blocking queue :
```java
import java.util.concurrent.atomic.AtomicInteger;
import java.util.*;

public class Solution {
    public static class MyQueue<T> {
        private List<T> queue = new ArrayList<>();
        private int maxSize;

        public MyBlockingQueue(int size) {
            this.maxSize = size;
        }

        public void remove() throws InterruptedException {
            synchronized (queue) {
                while (queue.isEmpty()) {
                    // there is nothing to remove, let's release the lock and let someone else add soMething in it first. I will go into "waiting" state
                    queue.wait();
                    // we put this inside a while loop because : to ensure that whenevEr we get a notification
                    // Whenever we get a notification , it comes from any thread and that too after a while. So we do not know if our check that we did in the while the condition above is even true anymore or not. Hence need to check it again once we get the lock.
                }
                System.out.println(System.currentTimeMillis() + " " + Thread.currentThread().getName() + " : " + "Popping an element out " + queue.remove(0));
                queue.notifyAll();

            }
        }

        public void add(T val) throws InterruptedException {
            synchronized (queue) {
                while (queue.size() >= maxSize) {
                    // there is not enough space to add, let's release the lock and let someone else add soMething in it first. I will go into "waiting" state
                    queue.wait();
                    // we put this inside a while loop because : to ensure that whenevEr we get a notification
                    // Whenever we get a notification , it comes from any thread and that too after a while. So we do not know if our check that we did in the while the condition above is even true anymore or not. Hence need to check it again once we get the lock.
                }
                System.out.println(System.currentTimeMillis() + " " + Thread.currentThread().getName() + " : " + "Adding to the queue " + val);
                queue.add(val);
                queue.notifyAll();
            }

        }

    }


    public static void main(String[] args) {
        MyBlockingQueue<Integer> myBlockingQueue = new MyBlockingQueue<Integer>(3);
        Thread t0 = new Thread(() -> {
            try {
                myBlockingQueue.add(1);


                myBlockingQueue.add(2);
                myBlockingQueue.add(3);
                myBlockingQueue.add(4);
                myBlockingQueue.add(5);
                myBlockingQueue.add(6);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });


        Thread t1 = new Thread(() -> {

            try {
                myBlockingQueue.remove();
                myBlockingQueue.remove();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        });

        t0.start();
        t1.start();

    }


}


```


#### Terminal Output :
```
1726571892258 Thread-0 : Adding to the queue 1
1726571892258 Thread-0 : Adding to the queue 2
1726571892258 Thread-0 : Adding to the queue 3
1726571892258 Thread-1 : Popping an element out 1
1726571892258 Thread-1 : Popping an element out 2
1726571892258 Thread-0 : Adding to the queue 4
1726571892258 Thread-0 : Adding to the queue 5

```








