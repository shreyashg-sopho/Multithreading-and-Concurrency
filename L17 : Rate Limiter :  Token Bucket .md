
### Token Bucket Rate Limiter :
A Token Bucket Rate Limiter is an algorithm used to control the rate at which actions or requests are allowed to occur. It ensures that a system can handle a controlled number of requests over time, helping to prevent overload or abuse of resources. It works by maintaining a "bucket" that fills up with tokens at a specified rate, and each request consumes one token.

#### Key Concepts:
**Capacity (Max Tokens):** The bucket can hold a maximum number of tokens, which defines how many requests can be handled at once.
**Fill Rate:** Tokens are added to the bucket at a steady rate over time (e.g., 1 token per second).
**Consumption:** Each request removes a token from the bucket. If there are no tokens left, the request must wait or be rejected.
#### Steps in Token Bucket Algorithm:
```
Tokens are added to the bucket at a regular rate (up to a certain maximum capacity).
When a request comes in, it checks if there are enough tokens:
If there are tokens available, one is removed, and the request is processed.
If there are no tokens, the request is either delayed until tokens are replenished or rejected.
```

### A simple implementation of token bucket (Forget Thread saftey for now)
```java
public class TokenBucketRateLimiter {
    private long maxCapacity;          // Maximum number of tokens the bucket can hold
    private long currentTokenCount;    // Current number of tokens in the bucket
    private long fillRate;             // Number of tokens added per refill

    public TokenBucketRateLimiter(long maxCapacity, long fillRate) {
        this.maxCapacity = maxCapacity;
        this.fillRate = fillRate;
        this.currentTokenCount = maxCapacity;  // Initialize bucket with full tokens
    }

    public boolean getToken() {
        if (currentTokenCount > 0) {
            currentTokenCount--;  // Consume a token
            return true;          // Token successfully retrieved
        }
        return false;             // No tokens available
    }

    public void addToken() {
        if (currentTokenCount + fillRate >= maxCapacity) {
            currentTokenCount = maxCapacity;  // Don't exceed the maximum capacity
        } else {
            currentTokenCount += fillRate;    // Add tokens based on the fill rate
        }
    }
}
```

In the above TokenBucketRateLimiter implementation, tokens are getting added at a constant rate (fillRate per second).



```java
public static void main(String[] args) {
        TokenBucketRateLimiter rateLimiter = new TokenBucketRateLimiter(2,1);
        Thread bucketFiller = new Thread(() ->
        {
            while(true)
            {
                rateLimiter.addToken();
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread requestCreator1 = new Thread(() ->
        {
            while(true)
            {
                boolean token = rateLimiter.getToken();
                if(token)
                    {System.out.println("Got the token");}
                else
                    {System.out.println("Did not get the token");}
                try {
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread requestCreator2 = new Thread(() ->
        {
            while(true)
            {
                boolean token = rateLimiter.getToken();
                if(token)
                {System.out.println("Got the token");}
                else
                {System.out.println("Did not get the token");}
                try {
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        bucketFiller.start();
        requestCreator1.start();
        requestCreator2.start();

    }
```
Thread "bucketFiller" is filling the bucket consistently in a second, while the "requestCreators" are requesting for a token time to time. They sometimes get it, otherwise they won't.

### Issues in the above code :
- The above code is not `thread safe`, two threads can access the same get token function and that could lead to the same token getting by two different threads. Therefore we must take lock when one thread is executing the function get token.
- Also design wise, this is not clean, the job of filling tokens constanly should be done in the TokenClass itself. 
Here is an improved version :

```java
public class TokenBucketRateLimiter {
    private long maxCapacity;                      // Maximum number of tokens the bucket can hold
    private long currentTokenCount;                // Current number of tokens in the bucket
    private long fillRate;                         // Number of tokens added per refill per second

   TokenBucketRateLimiter(long maxCapacity, long fillRate)
   {
    this.maxCapacity = maxCapacity;
    this.currentTokenCount = maxCapacity;
    this.fillRate = fillRate;
   }

   public void initialize()                       // Never start a thread in a constructor
   {
     Thread backgroundThread = new Thread(() -> {
         while(true)
         {
             this.addToken();
             try {
                 Thread.sleep(1000);
             } catch (InterruptedException e) {
                 e.printStackTrace();
             }
             System.out.println(currentTokenCount);
         }
     });
     backgroundThread.setDaemon(true);           // Make sure this thread doesn't block JVM shutdown
     backgroundThread.start();
   }

   public boolean getToken() throws InterruptedException {
       synchronized (this)
       {
           while(currentTokenCount == 0)
           {
               this.wait();
           }
           currentTokenCount--;
           return true;

       }
   }


 public void addToken()
    {
        synchronized (this) {
            if (currentTokenCount < maxCapacity) {
                currentTokenCount = Math.min(currentTokenCount + fillRate, maxCapacity);  // Ensure it doesn't exceed capacity
                this.notifyAll();  // Use notifyAll to wake up all waiting threads
            }
        }
    }


 public static void main(String[] args) {
        TokenBucketRateLimiter tokenBucketRateLimiter = new TokenBucketRateLimiter(2,1);
        tokenBucketRateLimiter.initialize();


        Thread requestCreator = new Thread(() ->
        {
            while(true)
            {
                boolean token = tokenBucketRateLimiter.getToken();
                if(token)
                {System.out.println("Got the token");}
                else
                {System.out.println("Did not get the token");}
                try {
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        requestCreator.start();


    }

}
```

### Note :
**1. Never start a thread in a constructor** :
Never start a thread in a constructor as the child thread can attempt to use the not-yet-fully constructed object using this. This is an `anti-pattern`.

**2. Why have we used the same object for locking ?** :
To avoid race condition. Use two diff lock objects and you will see race conditions.

**3. Can we use AtomicInteger or so instead of using synchronization here ?** :
Yes we can.
- AtomicInteger ensures thread-safe operations on integers without needing to use explicit locks or synchronization.
- It provides atomic methods like incrementAndGet(), decrementAndGet(), and get() that are crucial for the rate-limiting logic.
This reduces the overhead and complexity associated with synchronized blocks.




