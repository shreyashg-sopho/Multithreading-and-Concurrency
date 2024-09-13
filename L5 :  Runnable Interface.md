Runnable Interface : https://github.com/wangdapang77/Java8-Source-Code/blob/master/src/main/jdk8/java/lang/Runnable.java
Thread Class : https://github.com/wangdapang77/Java8-Source-Code/blob/a50cf6415182f1dd7e24ad48dd5d0303f11aeb23/src/main/jdk8/java/lang/Thread.java#L4


## Creating a Thread in Java : 

### Overridding Thread class's run method

```java
public class Threading0 {
    public static class Thread1 extends Thread{
        @Override
        public void run() {
          System.out.println("Jai Shree Ram by exetending Thread class");
        }
    }

    public static void main(String[] args) {
        Thread1 t1 = new Thread1();
        t1.start();
    }
}

```


### Using Runnable Interface : 
```java
public class Threading {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new Task());
        thread1.start();
        System.out.println("ThreadName : " + Thread.currentThread().getName());
    }

    static class Task implements Runnable{
        public void run() {
            System.out.println("ThreadName X: " + Thread.currentThread().getName());
        }
    }

}

```

### Explanation :
If we see (Thread implements Runnable interface), i.e Thread class itself is a **concrete implementaion** of Runnable interface. 

That means the Thread class itself must have an implementation for run () method. If we check that form the above github link we will find this to be true.
The impl is :
``` java

    @Override
    public void run() {
        if (target != null) {
            target.run();
        }
    }

// target here is the Runnable Impl passed in the Thread Constructor. if Nothing is passed, the run method will simply skip the condition and reach end.
```
So this means that we can either :
1. Initiate a thread either by implementing a Runnable and passing it as construtor to the Thread Object. 
2. Or We can also override the run method of the Thread object to do whatever we want !!! 


## Why is there two ways needed ?
This is because in Java multiple inheritance is not allowed (Can;t extend multiple classes at once), but one class can implement multiple interfaces. 

## Conclusion : 
So if we go the Extend/Mehtod Overriding way (like we did above). Thread1 class cannot do any inheritance.
While if we go the implementing runnable interface way and passing that to the Thread way, we can still do inheritance. HENCE THIS IS A BETTER WAY AS WE HAVE LESS CONSTRAINTS IN THIS WAY. 
























