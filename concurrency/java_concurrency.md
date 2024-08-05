### Common Concurrency and Multithreading Questions and Answers

#### 1. What is the difference between concurrency and parallelism?

**Answer**: 
- **Concurrency** is the ability of a system to handle multiple tasks at the same time by interleaving their execution. Tasks might not run simultaneously but are managed in such a way that it appears they are running concurrently.
- **Parallelism** is the simultaneous execution of multiple tasks, usually on multiple processors or cores. Tasks actually run at the same time.

#### 2. What are threads and how do they differ from processes?

**Answer**: 
- **Threads** are the smallest unit of execution within a process. They share the same memory space and resources of the process.
- **Processes** are independent execution units with their own memory space. Multiple threads within the same process can communicate easily, whereas inter-process communication is more complex.

#### 3. What is a race condition, and how can it be prevented?

**Answer**: 
- A **race condition** occurs when two or more threads access shared data and try to change it at the same time. The final outcome depends on the sequence of accesses, leading to unpredictable results.
- It can be prevented using synchronization mechanisms such as **locks**, **mutexes**, **semaphores**, and **synchronized blocks** to ensure that only one thread accesses the critical section of code at a time.

#### 4. What is deadlock and how can it be avoided?

**Answer**: 
- **Deadlock** occurs when two or more threads are waiting indefinitely for resources held by each other, causing all of them to be blocked.
- It can be avoided by:
  - **Resource ordering**: Always acquiring resources in a predefined order.
  - **Timeouts**: Implementing timeouts when acquiring locks.
  - **Deadlock detection**: Periodically checking for cycles in the resource allocation graph.
  - **Avoid holding locks**: Keeping the lock holding time to a minimum.

#### 5. What is a mutex, and how does it work?

**Answer**: 
- A **mutex** (mutual exclusion) is a synchronization primitive that ensures that only one thread can access a resource at a time.
- When a thread locks a mutex, other threads that try to lock it will be blocked until the mutex is unlocked. This ensures serialized access to the shared resource.

#### 6. What are semaphores, and how are they different from mutexes?

**Answer**: 
- **Semaphores** are signaling mechanisms that control access to a shared resource by multiple threads. They can be used to allow a certain number of threads to access a resource concurrently.
- **Mutexes** are binary semaphores with a value of 0 or 1, used for mutual exclusion. Semaphores can have a value greater than one, allowing multiple threads to enter the critical section simultaneously.

#### 7. What is a thread pool and why is it useful?

**Answer**: 
- A **thread pool** is a collection of pre-instantiated reusable threads that can be used to execute tasks.
- It is useful because it reduces the overhead of creating and destroying threads frequently, improves performance by reusing threads, and helps in managing the number of concurrent threads to optimize resource usage.

#### 8. Explain the difference between synchronized methods and synchronized blocks in Java.

**Answer**: 
- **Synchronized methods** lock the entire method, ensuring that only one thread can execute it at a time. This can lead to performance bottlenecks if the method contains non-critical sections.
- **Synchronized blocks** allow more fine-grained control by locking only a specific section of code, reducing contention and improving performance.

**Example**:
```java
public synchronized void synchronizedMethod() {
    // entire method is synchronized
}

public void methodWithSynchronizedBlock() {
    synchronized (this) {
        // only this block is synchronized
    }
}
```

#### 9. What is a volatile keyword in Java, and when would you use it?

**Answer**: 
- The **volatile** keyword in Java is used to indicate that a variable's value will be modified by different threads. It ensures that changes to the variable are visible to all threads.
- It is used when a variable is shared between multiple threads and changes frequently, ensuring visibility and preventing caching of the variable.

**Example**:
```java
private volatile boolean flag = true;

public void setFlag(boolean value) {
    flag = value;
}

public boolean getFlag() {
    return flag;
}
```

#### 10. What is the purpose of the `wait()`, `notify()`, and `notifyAll()` methods in Java?

**Answer**: 
- **`wait()`**: Causes the current thread to wait until another thread invokes `notify()` or `notifyAll()` on the same object.
- **`notify()`**: Wakes up a single thread that is waiting on the object's monitor.
- **`notifyAll()`**: Wakes up all threads that are waiting on the object's monitor.

These methods are used for inter-thread communication, ensuring that threads can cooperate by waiting for conditions to be met before proceeding.

**Example**:
```java
synchronized (lock) {
    while (condition) {
        lock.wait();
    }
    // perform action
    lock.notify();
}
```

### Further Concurrency and Multithreading Topics

#### 11. Explain the concept of a "thread-safe" class.

**Answer**: 
- A **thread-safe** class ensures that its methods and properties can be accessed by multiple threads concurrently without leading to inconsistent states or race conditions.
- It is achieved through synchronization mechanisms like synchronized methods, synchronized blocks, locks, and atomic variables.

#### 12. What is the `Callable` interface, and how is it different from `Runnable`?

**Answer**: 
- **`Callable`**:
  - Returns a result and can throw a checked exception.
  - Defined in `java.util.concurrent`.
  ```java
  public interface Callable<V> {
      V call() throws Exception;
  }
  ```

- **`Runnable`**:
  - Does not return a result and cannot throw a checked exception.
  - Defined in `java.lang`.
  ```java
  public interface Runnable {
      void run();
  }
  ```

#### 13. What are the main differences between `synchronized` and `Lock` in Java?

**Answer**: 
- **`synchronized`**:
  - Simple to use and applies to method or block.
  - Implicitly handles lock acquisition and release.
  - Cannot be interrupted while waiting for the lock.

- **`Lock`**:
  - More flexible and provides additional features (e.g., `ReentrantLock`, `ReadWriteLock`).
  - Requires explicit lock acquisition and release.
  - Can be interrupted while waiting for the lock and can attempt to acquire the lock with a timeout.

**Example**:
```java
Lock lock = new ReentrantLock();
lock.lock();
try {
    // critical section
} finally {
    lock.unlock();
}
```

#### 14. What is the `ForkJoinPool` and how does it work?

**Answer**: 
- **`ForkJoinPool`** is a specialized implementation of the ExecutorService interface designed for work that can be broken into smaller tasks and executed concurrently.
- It uses a work-stealing algorithm to balance the load among threads, improving performance for divide-and-conquer algorithms.

**Example**:
```java
ForkJoinPool pool = new ForkJoinPool();
pool.invoke(new RecursiveTask<Integer>() {
    @Override
    protected Integer compute() {
        // task logic
    }
});
```

#### 15. Explain the concept of a "daemon thread" in Java.

**Answer**: 
- A **daemon thread** is a background thread that does not prevent the JVM from exiting when all user threads have finished execution.
- It is used for tasks like garbage collection and background monitoring.

**Example**:
```java
Thread daemonThread = new Thread(() -> {
    // daemon task
});
daemonThread.setDaemon(true);
daemonThread.start();
```

## Additional Commonly Asked Concurrency and Multithreading Questions

#### 1. Explain the concept of thread starvation and how it can be prevented.
**Answer**:
- **Thread Starvation** occurs when a thread is perpetually denied access to resources it needs for execution because other threads are continuously given priority.
- **Prevention**: 
  - Use fair locks (e.g., `ReentrantLock` with fairness set to true).
  - Avoid high-priority threads monopolizing the CPU.
  - Implement resource scheduling policies that ensure all threads get a chance to execute.

#### 2. What is the difference between `wait()` and `sleep()` in Java?
**Answer**:
- **`wait()`**: 
  - Used for inter-thread communication.
  - Releases the lock on the object, allowing other threads to acquire it.
  - Must be called from a synchronized context.
- **`sleep()`**:
  - Pauses the current thread for a specified time.
  - Does not release the lock.
  - Can be called from any context.

#### 3. What is a `CyclicBarrier` and how does it work?
**Answer**:
- A **`CyclicBarrier`** is a synchronization aid that allows a set of threads to all wait for each other to reach a common barrier point.
- Useful for scenarios where threads must wait at a certain point before proceeding.
```java
CyclicBarrier barrier = new CyclicBarrier(3, () -> System.out.println("Barrier reached"));
Runnable task = () -> {
    try {
        System.out.println(Thread.currentThread().getName() + " waiting");
        barrier.await();
        System.out.println(Thread.currentThread().getName() + " proceeding");
    } catch (InterruptedException | BrokenBarrierException e) {
        e.printStackTrace();
    }
};
```

#### 4. Describe the `CountDownLatch` and its typical use case.
**Answer**:
- A **`CountDownLatch`** is a synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads completes.
- Useful for ensuring that a task waits for the completion of other tasks.
```java
CountDownLatch latch = new CountDownLatch(3);
Runnable task = () -> {
    try {
        // Perform task
    } finally {
        latch.countDown();
    }
};
```

#### 5. What is a `Phaser` in Java, and how does it differ from `CountDownLatch` and `CyclicBarrier`?
**Answer**:
- A **`Phaser`** is a more flexible synchronization barrier that supports multiple phases.
- Unlike `CountDownLatch` (one-time use) and `CyclicBarrier` (fixed number of threads), a `Phaser` can be dynamically adjusted to add or remove parties.
```java
Phaser phaser = new Phaser(1);
phaser.register();
phaser.arriveAndAwaitAdvance();
phaser.arriveAndDeregister();
```

#### 6. Explain `ThreadLocal` and its use cases.
**Answer**:
- **`ThreadLocal`** provides thread-local variables, which are variables that each thread accessing them has its own, independently initialized copy.
- Useful for maintaining per-thread context (e.g., user sessions, database connections).
```java
ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);
threadLocal.set(threadLocal.get() + 1);
```

#### 7. What is a `ReentrantReadWriteLock`, and when would you use it?
**Answer**:
- A **`ReentrantReadWriteLock`** allows multiple threads to read a resource concurrently but only one thread to write to it at a time.
- Useful for scenarios with frequent read operations and infrequent write operations.
```java
ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();
rwLock.readLock().lock();
try {
    // Read operation
} finally {
    rwLock.readLock().unlock();
}
rwLock.writeLock().lock();
try {
    // Write operation
} finally {
    rwLock.writeLock().unlock();
}
```

#### 8. What is the `ForkJoinPool` and how does it improve performance?
**Answer**:
- **`ForkJoinPool`** is a specialized thread pool for parallel processing, breaking tasks into smaller subtasks.
- It uses work-stealing to balance the load, improving performance for divide-and-conquer algorithms.
```java
ForkJoinPool pool = new ForkJoinPool();
pool.invoke(new RecursiveTask<Integer>() {
    @Override
    protected Integer compute() {
        // Divide and conquer task
    }
});
```

#### 9. Explain the concept of "lock-free" algorithms.
**Answer**:
- **Lock-free algorithms** ensure that at least one thread makes progress in a finite number of steps, avoiding the pitfalls of locking (e.g., deadlocks, thread contention).
- They often use atomic operations and are highly efficient for certain types of concurrent programming.

#### 10. How can you detect and diagnose deadlocks in a Java application?
**Answer**:
- **Detection**: Use tools like jstack, VisualVM, or thread dump analyzers to detect threads waiting indefinitely.
- **Diagnosis**: Analyze the thread dump to identify the locked resources and threads involved in the deadlock.
