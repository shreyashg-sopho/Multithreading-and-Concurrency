### Volatile in Java
The volatile keyword in Java is used to mark a variable as being stored in main memory. It ensures visibility and consistency of the variable's value across multiple threads. This keyword is essential in multi-threaded environments where different threads may access and modify shared variables.

### Use Case of volatile:
In Java, each thread typically has its own cache, and variables may be stored locally in the thread's cache for performance reasons. This can lead to a situation where:

One thread updates a variable in its cache.
Another thread doesn't see the updated value because it's still reading from its own cached version.
By marking a variable as volatile, you tell the JVM:

No Caching: The variable is not to be cached thread-locally. Every read and write to this variable will happen directly from and to the main memory.
Visibility: When one thread modifies a volatile variable, the change is visible to all other threads immediately.

### Applications of volatile:
#### 1.Flags and States: 
It is commonly used for variables that act as flags or states between threads. For example, a boolean flag to signal shutdown:


##### Code : 




