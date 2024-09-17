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


```




