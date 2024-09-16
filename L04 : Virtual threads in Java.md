
## Virtual Threads (Project Loom)

### Definition: 
Introduced as a preview feature in Java 19 , virtual threads are lightweight threads that can scale better than traditional threads.

### Characteristics:

Runn on top of platform threads (a standard Java thread), allowing for massive concurrency.

#### In simpler terms :
```
In previous versions of Java : 
1 Platform Thread == 1 OS thread.

In 19+ versions of Java :
1 Platform Thread == 1 OS thread.
But 100s of virtual threads can be ampped to one PLatform JVM thread. which corressponds to :
100s of Virtual Threads = 1 Platform Thread == 1 OS thread.


```



### Example:

```java

Thread.startVirtualThread(() -> {
    System.out.println("Virtual Thread running");
});
```

### Experiments : 

#### Case 1 : Using Fixed Threadpool

``` java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.stream.IntStream;

class Timepass2{
    public static void main(String[] args) {
        long begin = System.currentTimeMillis();
        ExecutorService executor = Executors.newFixedThreadPool(2000);

            IntStream.range(0, 2000).forEach(i -> {
                executor.submit(() -> {
                    Thread.sleep(1);
                    return i;
                });
            });

          // executor.close() is called implicitly, and waits

        long end = System.currentTimeMillis();
        System.out.println((end - begin));
    }
}
``` 
#### Output : 
```
244
```
### Explanation  : 
The JVM here tried to create 100000 threads upfront even before executing a single request, which corressponds to 100000 OS threads and this is taking super long time to do. Not at all the right way to do.


#### Case 2 : Using Cached Threadpool

``` java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.stream.IntStream;

class Timepass2{
    public static void main(String[] args) {
        long begin = System.currentTimeMillis();
        ExecutorService executor = Executors.newFixedThreadPool(2000);

            IntStream.range(0, 2000).forEach(i -> {
                executor.submit(() -> {
                    Thread.sleep(1);
                    return i;
                });
            });

          // executor.close() is called implicitly, and waits

        long end = System.currentTimeMillis();
        System.out.println((end - begin));
    }
}
``` 
#### Output : 
```
110
```
The JVM here will create the threadpool and accordingly start running the requests submitted to ES. Whenevre a request comes and there is no existing thread in the thredpool to take the request, a new thread will be spawned. Since the threads in threadpool are getting created later unlike the above, hence lesser threads in total are spawned and takes less time to create all threads. 


#### Case 3 : Using Virtual Threadpool
``` java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.stream.IntStream;

class Timepass2{
    public static void main(String[] args) {
        long begin = System.currentTimeMillis();
        ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();

            IntStream.range(0, 2000).forEach(i -> {
                executor.submit(() -> {
                    Thread.sleep(1);
                    return i;
                });
            });

          // executor.close() is called implicitly, and waits

        long end = System.currentTimeMillis();
        System.out.println((end - begin));
    }
}
```
In terms of creating threads, this will be faster than the CacheThreadPool and hence can submnit more tasks quickly. 
This is what the usecase of Virtual threads is that it will help us create a large number of lightweitgh virtual threads quickly. 


Note that the increase is only in terms of throughput not in terms of latency.
That is a large number of threads will be created but execution time mei not much diff will be observed. 


Since Virtual threads are relatively a new construct, hence this is still not a go thing as the industry is still figuring out the best practices yet.











