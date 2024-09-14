## What is synchronised keyword ?

If multiple threads are trying to operate simultaneously on the same java object then there may be a chance of data inconsistency problem. To overcome this problem we should go for the `synchronized` keyword, then at the time only one thread is allowed to execute that method or block on the given object so that data inconsistency problem will be resolved.

The main advantage of the `synchronized` keyword is we can resolve the data inconsistency problem but the main disadvantage of the `synchronized` keyword is, it increases waiting time thread and creates performance problem, hence it is not recommended to use synchronized keyword.

If a thread wants to execute the synchronized method in the given object:
-  it has to get a lock of that object
-  once thread got the lock then it is allowed to execute any synchronized method on that object.
-  once method execution completes automatically thread releases lock.
when a thread executing a synchronized method on the given object the remaining thread is not allowed to execute simultaneously on the same object but the remaining thread is allowed to execute a non-synchronized method simultaneously.

### Learning by Example : 
#### Problem statement :
Let's take an example that we have a Resting Place, the person coming in first will get to take rest for x units of time, the next will get for 2x and the next for 4x and so on. And at any given instance, only one person can occupy the Resting place.

#### Solution 1 (Plain Java Code)

```java
public class SomeClass {


    public static class RestingPlace
    {
        Long mils = 2000l;
        private void  takeRest () throws InterruptedException {
            System.out.println("I am " + Thread.currentThread().getName() + " and I will be  sleeping and resting for " + mils + " ms");
            Thread.sleep(mils);
            System.out.println("I am " + Thread.currentThread().getName() + " and I want the next to sleep 2x of what I slept i.e for " + mils*2);
            mils *=2;

        }
    }

    public static void main(String[] args) {
        RestingPlace restingPlace = new RestingPlace();

        Runnable runnable =  () -> {
            try {
                restingPlace.takeRest();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        };
        Thread tim = new Thread(runnable, "Tim");
        Thread david = new Thread(runnable, "David");
        tim.start();
        david.start();
    }
}
```
#### Terminal Output of the above code :
```
I am Tim and I will be  sleeping and resting for 2000 ms
I am David and I will be  sleeping and resting for 2000 ms
I am David and I want the next to sleep 2x of what I slept i.e for 4000
I am Tim and I want the next to sleep 2x of what I slept i.e for 4000
```
The two threads shared the same instance of Resting Place (just as we wanted). 
But both Tim and David Thread accessed the same piece of code, instead of just being one at a time.
Not only did that broke our constraint that only one should be resting at once, but also that both of them slept for x and x units respectively instead of x and 2x.
This happened because our code is not thread safe and the takeRest method is accessible by both the Threads at the same time, which is also leading to shared variables getting read/write wrongly. (If there were any 3rd person to access the room, it shoul dbe sleeping for 4x i.e 8000 as per above code but our code has printed that the next person will sleep only for 4000).

This could be fixed if only we could allow only one piece of code to take access of take rest method. 


#### Solution 2 (Writing a thread safe code)
```java
public class SomeClass {


    public static class RestingPlace
    {
        Long mils = 2000l;
        private void synchronized takeRest () throws InterruptedException {
            System.out.println("I am " + Thread.currentThread().getName() + " and I will be  sleeping and resting for " + mils + " ms");
            Thread.sleep(mils);
            System.out.println("I am " + Thread.currentThread().getName() + " and I want the next to sleep 2x of what I slept i.e for " + mils*2);
            mils *=2;

        }
    }

    public static void main(String[] args) {
        RestingPlace restingPlace = new RestingPlace();

        Runnable runnable =  () -> {
            try {
                restingPlace.takeRest();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        };
        Thread tim = new Thread(runnable, "Tim");
        Thread david = new Thread(runnable, "David");
        tim.start();
        david.start();
    }
}
```
#### Terminal Output of the above code :
```
I am Tim and I will be  sleeping and resting for 2000 ms
I am Tim and I want the next to sleep 2x of what I slept i.e for 4000
I am David and I will be  sleeping and resting for 4000 ms
I am David and I want the next to sleep 2x of what I slept i.e for 8000
```


Now that we have declared the method to with `synchronized` keyword, which means that if one thread has already the access to the takeRest method, other threads cannot access the same block of code. (Just as what we wanted for our problem statement's correct solution)


### Conclusion :
Synchronization is an essential mechanism in Java for ensuring that shared resources are accessed in a thread-safe manner. It helps prevent race conditions by allowing only one thread to execute critical sections of the code at a time. 


