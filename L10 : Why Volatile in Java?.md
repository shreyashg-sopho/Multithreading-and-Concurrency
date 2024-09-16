## Volatile Keyword.

Let's take the following example : 
The task is to run a loop until the value of flag is true. As soon as it is false : 
- The print statement should false if the value has been updated as false by then. 
- The while loop must not execute if the value is false.

#### Sample Code

```java
public class SimulatingIssuesWithNonVolatile {
    private static boolean isRunning = true; // Non-volatile flag

    public static void main(String[] args) throws InterruptedException {
        // Task is A loop that runs based on isRunning flag
        Runnable task = () -> {
            while (isRunning) {
                System.out.println("Value of is running in thread " + Thread.currentThread().getName() +" is " + isRunning);
            }
        };
        Thread threadA = new Thread(task, "A");
        Thread threadB = new Thread(task, "B");
        Thread threadC = new Thread(task, "C");
        Thread threadD = new Thread(task, "D");
        Thread threadE = new Thread(task, "E");
        Thread threadF = new Thread(task, "F");
        // Start child thread
        threadA.start();
        threadB.start();
        threadC.start();
        threadD.start();
        threadE.start();
        threadF.start();

        // Sleep for 10 millis
        Thread.sleep(10);


        isRunning = false; // But Thread B might not see this change!

        System.out.println("isRunning set to false.");
    }
}
```

Terminal Output : 
```
Value of is running in thread A is true
Value of is running in thread A is true
Value of is running in thread A is true
Value of is running in thread A is true
Value of is running in thread C is true
Value of is running in thread E is true
Value of is running in thread C is true
Value of is running in thread B is true
Value of is running in thread B is true
Value of is running in thread F is true
Value of is running in thread D is true
Value of is running in thread F is true
Value of is running in thread B is true
Value of is running in thread F is true
Value of is running in thread F is true
.
.
.
.
.
isRunning set to false.
Value of is running in thread C is true
Value of is running in thread F is true
Value of is running in thread B is true
Value of is running in thread A is true
Value of is running in thread D is true
```


#### Issue with the above code : 
Typically the value of isRunning ince set to false,
- The while loop should not execute.
- Even if it executed, the value of isRunning in print statement should not be true, rather false should be printed. 

But that did not happen because we each thread is reading from it's own cache which is not uptodate with the value written in RAM or some one else's latest write in cache


```
INITIALLY : 

           -----------------------         -----------------------         -----------------------
           |     Thread A         |       |     Thread B          |        |     Main Thread       |
           |                      |       |                       |        |                       |
           | CACHE (flag = true)  |       |   CACHE (flag = true) |        |   CACHE (flag = true) |
           ------------------------        ------------------------        ------------------------

                       ^                                ^                                 ^
                       |                                |                                 |
                       |                                |                                 |
                       |                      -----------------------                     |
                        -------------------- |    Main Memory       |---------------------
                                             |    int flag = true   |
                                             ------------------------

AFTER UPDATION BY MAIN THREAD (Only done in local cache though): 


           -----------------------         -----------------------         -----------------------
           |     Thread A         |       |     Thread B          |        |     Main Thread       |
           |                      |       |                       |        |                       |
           | CACHE (flag = true)  |       |   CACHE (flag = true) |        |   CACHE (flag = false)|
           ------------------------        ------------------------        ------------------------

                       ^                                ^                                 ^
                       |                                |                                 |
                       |                                |                                 |
                       |                      -----------------------                     |
                        -------------------- |    Main Memory       |---------------------
                                             |    int flag = true   |
                                             ------------------------

AFTER GETTING UPADTED IN RAM :

           -----------------------         -----------------------         -----------------------
           |     Thread A         |       |     Thread B          |        |     Main Thread       |
           |                      |       |                       |        |                       |
           | CACHE (flag = true)  |       |   CACHE (flag = true) |        |   CACHE (flag = false)|
           ------------------------        ------------------------        ------------------------

                       ^                                ^                                 ^
                       |                                |                                 |
                       |                                |                                 |
                       |                      -----------------------                     |
                        -------------------- |    Main Memory       |---------------------
                                             |    int flag = false  |
                                             ------------------------

VALUE GOT UPDATED IN A's cache but not in B's yet.


            -----------------------         -----------------------         -----------------------
           |     Thread A         |       |     Thread B          |        |     Main Thread       |
           |                      |       |                       |        |                       |
           | CACHE (flag = false) |       |   CACHE (flag = false)|        |   CACHE (flag = false)|
           ------------------------        ------------------------        ------------------------

                       ^                                ^                                 ^
                       |                                |                                 |
                       |                                |                                 |
                       |                      -----------------------                     |
                        -------------------- |    Main Memory       |---------------------
                                             |    int flag = false  |
                                             ------------------------


VALUE FINALLY GOT UPDATED FOR ALL :

VALUE GOT UPDATED IN A's cache but not in B's yet.


           -----------------------         -----------------------         -----------------------
           |     Thread A         |       |     Thread B          |        |     Main Thread       |
           |                      |       |                       |        |                       |
           | CACHE (flag = false) |       |   CACHE (flag = false)|        |   CACHE (flag = false)|
           ------------------------        ------------------------        ------------------------

                       ^                                ^                                 ^
                       |                                |                                 |
                       |                                |                                 |
                       |                      -----------------------                     |
                        -------------------- |    Main Memory       |---------------------
                                             |    int flag = false  |
                                             ------------------------


```


### Conclusion
All of this happened because everyone is reading and updating in their own cache.


