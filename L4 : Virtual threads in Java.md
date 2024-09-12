
## Virtual Threads (Project Loom)

### Definition: 
Introduced as a preview feature in Java 19 , virtual threads are lightweight threads that can scale better than traditional threads.

### Characteristics:

Runn on top of platform threads, allowing for massive concurrency.

### Example:

```java
Copy code
Thread.startVirtualThread(() -> {
    System.out.println("Virtual Thread running");
});
```

### Experiments : 

#### Case 1 : Using Fixed Threadpool

``` java
import java.time.Duration;
import java.time.Instant;
import java.util.concurrent.Executors;
import java.util.stream.IntStream;

public class Main{
    public static void main(String[] args) {
        var begin = Instant.now();
        try (var executor = Executors.newFixedThreadPool(100000)) {
            IntStream.range(0, 100000).forEach(i -> {
                executor.submit(() -> {
                    Thread.sleep(Duration.ofSeconds(1));
                    return i;
                });
            });
        }  // executor.close() is called implicitly, and waits

        var end = Instant.now();
        System.out.println(Duration.between(end, begin));
    }
}
``` 
#### Output : 
```
PT5010.289483S
```
### Explanation  : 
The JVM here tried to create 100000 threads upfront even before executing a single request, which corressponds to 100000 OS threads and this is taking super long time to do. Not at all the right way to do.


#### Case 2 : Using Cached Threadpool

``` java
import java.time.Duration;
import java.time.Instant;
import java.util.concurrent.Executors;
import java.util.stream.IntStream;

public class Main{
    public static void main(String[] args) {
        var begin = Instant.now();
        try (var executor = Executors.newCachedThreadPool()) {
            IntStream.range(0, 100000).forEach(i -> {
                executor.submit(() -> {
                    Thread.sleep(Duration.ofSeconds(1));
                    return i;
                });
            });
        }  // executor.close() is called implicitly, and waits

        var end = Instant.now();
        System.out.println(Duration.between(end, begin));
    }
}
``` 
#### Output : 
```
PT5.289483S
```





