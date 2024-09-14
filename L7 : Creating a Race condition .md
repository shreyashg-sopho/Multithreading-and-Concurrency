## Race Condition 

In a multi-threaded environment, when multiple threads try to access and modify shared data concurrently, it may lead to unpredictable results, also known as a **race condition**. In the context of our stack implementation, this means that if multiple threads try to push or pop elements from the stack simultaneously, the stack could end up in an inconsistent state.

## Race Condition Example in Stack

Let's modify our stack implementation to illustrate a race condition:

```java
public class Stack {
    private int[] stackArray;
    private int top;
    private int maxSize;

    public Stack(int size) {
        this.maxSize = size;
        this.stackArray = new int[maxSize];
        this.top = -1;  // Stack is initially empty
    }

    public void push(int value) {
        // Simulating a race condition
        if (isFull()) {
            System.out.println("Stack is full! Cannot push " + value);
        } else {
            top = top + 1;
            stackArray[top] = value;
            System.out.println("Pushed: " + value);
        }
    }

    public int pop() {
        if (isEmpty()) {
            System.out.println("Stack is empty! Cannot pop");
            return -1;
        } else {
            return stackArray[top--];
        }
    }

    public boolean isEmpty() {
        return top == -1;
    }

    public boolean isFull() {
        return top == maxSize - 1;
    }
}
```

Now Simulating multiple threads pushing and popping concurrently
``` java
public class RaceConditionTesting  {
    public static void main(String[] args) {
        Stack myStack = new Stack(5);

        // Thread 1 trying to push elements
        Thread thread1 = new Thread(() -> {
            myStack.push(10);
            myStack.push(20);
            myStack.push(30);
            myStack.push(40);
        });

        // Thread 2 trying to pop elements
        Thread thread2 = new Thread(() -> {
            myStack.pop();
            myStack.pop();
            myStack.pop();
            myStack.pop();
        });

        thread1.start();
        thread2.start();
    }
}
```

### Race condition that happened above:

Two threads (thread1 and thread2) are working on the same stack instance concurrently. thread1 is pushing data while thread2 is popping data.
Since the push() and pop() methods are not synchronized, it’s possible for one thread to modify the top value while another thread is accessing it, leading to unexpected behavior like incorrect top values or corrupted stack data.
Race Condition Scenarios:
- Thread 1 checks if the stack is full and proceeds to push an element.
- Before Thread 1 completes the push operation, Thread 2 pops an element, altering the top index.
- This causes an inconsistency, as Thread 1 still assumes the previous value of top, leading to incorrect stack behavior.

Many such scenarios can happen. Assume two different thread try to push when these is only one space left in the stack. Both might read top = mex - 1 but only one will be able to push it while the other will get index out of bound error.


Thus, we can conclude that any piece of code that is being executed by 2 threads concurrently and that modifies any shared variable is not `thread safe`. 
