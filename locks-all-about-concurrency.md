### 1. What is a ReentrantLock?

A **ReentrantLock** is a class in Java’s `java.util.concurrent.locks` package. It’s essentially a more flexible version of the implicit lock you get when using the `synchronized` keyword.

Key points:

* **Reentrant**: The same thread can acquire the lock multiple times without deadlocking itself. Each acquisition increases a counter, and each `unlock()` call decreases it. The lock is released only when the counter reaches zero.
* **Explicit locking**: Unlike `synchronized` blocks, you manually call `lock()` and `unlock()`. This gives you more control, e.g., try-locking or interruptible locks.
* **Ownership**: Only the thread that acquired the lock can release it. If another thread tries to unlock it, Java throws `IllegalMonitorStateException`.




## **1. Key concept of ReentrantLock**

* A **ReentrantLock** allows **the same thread** to acquire the lock multiple times without deadlocking itself.
* **Ownership matters**: Only the thread that acquired the lock can release it.
* **Lock count**: Every time a thread locks, an internal counter increases. Unlocking decreases it. The lock is fully released only when the counter reaches zero.

---

## **2. Use Case Example: Bank Account Transfers**

Imagine we have a **BankAccount** class with `deposit()` and `transfer()` methods. Multiple threads may try to transfer money simultaneously.

```java
import java.util.concurrent.locks.ReentrantLock;

class BankAccount {
    private int balance;
    private ReentrantLock lock = new ReentrantLock();

    public BankAccount(int balance) {
        this.balance = balance;
    }

    public void deposit(int amount) {
        lock.lock();
        try {
            balance += amount;
            System.out.println(Thread.currentThread().getName() + " deposited: " + amount);
        } finally {
            lock.unlock();
        }
    }

    public void transfer(BankAccount to, int amount) {
        lock.lock();  // acquire lock for this account
        try {
            System.out.println(Thread.currentThread().getName() + " transferring: " + amount);
            deposit(-amount);  // re-enters lock safely
            to.deposit(amount); // lock in another account separately
        } finally {
            lock.unlock();
        }
    }

    public int getBalance() {
        return balance;
    }
}
```

---

### **3. How it works with multiple threads**

1. **Thread A** wants to transfer \$100 from Account1 to Account2.

   * It calls `transfer()`.
   * Thread A acquires `lock` on Account1.

2. Inside `transfer()`, it calls `deposit(-amount)`.

   * Since `deposit()` also locks the **same ReentrantLock**, Thread A can **re-enter the lock** because it’s the same thread.
   * Lock counter increases by 1.

3. Thread A finishes `deposit()` inside `transfer()`.

   * Unlock decreases counter by 1.
   * Thread A still holds the lock for `transfer()`.

4. After finishing `transfer()`, unlock decreases counter to 0.

   * Now the lock is fully released, and **other threads can acquire it**.

5. **Thread B** trying to access Account1 must **wait** until Thread A fully releases the lock.

---

### **4. Key Points from This Example**

* **Reentrancy** prevents deadlock when the same thread calls multiple methods that lock the same object.
* **Other threads must wait** until the lock is fully released.
* You can lock/unlock in **different methods**, as long as the **same thread** performs the unlocking.
* Unlocking from a **different thread** will throw `IllegalMonitorStateException`.

---

### **5. Output Example (Conceptual)**

```
Thread-1 transferring: 100
Thread-1 deposited: -100
Thread-1 deposited: 100
Thread-2 deposited: 50  <-- runs only after Thread-1 fully releases the lock
```

Here you see **Thread-2 waits** until Thread-1 completely unlocks.

---

In short:

> ReentrantLock allows a thread to “re-enter” safely while other threads **cannot access the locked resource** until it is fully released. It’s perfect for nested method calls and complex workflows where multiple methods share the same critical section.

---

---

### 2. Why it’s “just like a mutex”

A **mutex** (mutual exclusion lock) is a low-level synchronization primitive in threading:

* It allows **only one thread** to access a critical section at a time.
* Other threads attempting to acquire the mutex must **wait** until it is released.
* The **same thread cannot release a mutex it doesn’t own**.

The behavior of **ReentrantLock** mirrors these properties:

| Feature          | ReentrantLock                         | Traditional Mutex                                                                     |
| ---------------- | ------------------------------------- | ------------------------------------------------------------------------------------- |
| Mutual exclusion | ✅ Only one thread can hold it         | ✅ Same                                                                                |
| Ownership        | ✅ Only owning thread can unlock       | ✅ Same                                                                                |
| Reentrancy       | ✅ Same thread can lock multiple times | ❌ Usually not reentrant unless specifically designed (like PTHREAD\_RECURSIVE\_MUTEX) |
| Flexibility      | ✅ Lock/Unlock in different methods    | ❌ Usually lock/unlock in same scope                                                   |

So, **ReentrantLock is basically a Java mutex with reentrancy and extra features**.

---

### 3. Example in Java

```java
import java.util.concurrent.locks.ReentrantLock;

public class Example {
    private ReentrantLock lock = new ReentrantLock();

    public void methodA() {
        lock.lock();
        try {
            System.out.println("In methodA");
            methodB(); // Safe to call; same thread can re-lock
        } finally {
            lock.unlock();
        }
    }

    public void methodB() {
        lock.lock();
        try {
            System.out.println("In methodB");
        } finally {
            lock.unlock();
        }
    }
}
```

Notice:

* `methodA` calls `methodB`. The **same thread** can acquire the lock multiple times without deadlock.
* If a **different thread** tries `lock.unlock()`, it will throw `IllegalMonitorStateException` (just like pthread mutexes).

---

### 4. Comparison to `pthread_mutex_t` in C

* **pthread\_mutex\_t** is a classic mutex.
* You can make it **reentrant** with `PTHREAD_MUTEX_RECURSIVE`.
* Behavior: Only the owning thread can unlock; trying to unlock from a different thread causes undefined behavior or an error.

**So conceptually:**

> Java ReentrantLock ≈ pthread recursive mutex with extra Java API features like tryLock(), lockInterruptibly(), fairness policies, and condition variables.

---

### ✅ TL;DR

* **ReentrantLock** = explicit, reentrant mutex in Java.
* Like a mutex, it ensures mutual exclusion and ownership rules.
* Unlike `synchronized`, you can lock and unlock in **different methods**.
* Unlocking from a non-owning thread throws **IllegalMonitorStateException**, just like a mutex would error.
* It provides more flexibility than Java’s implicit monitor locks (`synchronized`), making it suitable for complex locking scenarios.

---


# **1. Implicit Lock (Monitor Lock)**

* **Definition**: Locks that are automatically associated with a block of code or an object.
* **Example in Java**: `synchronized` keyword.
* **How it works**:

  * When a thread enters a `synchronized` method or block, it **automatically acquires the lock** on the object (or class for static methods).
  * The lock is released automatically when the block exits (even if exceptions occur).
* **Pros**: Simple, safe (automatic release).
* **Cons**: Less flexible (cannot try to lock, no timeout, cannot lock in one method and unlock in another).

```java
synchronized(this) {
    // critical section
}
```

---

## **2. Explicit Lock**

* **Definition**: Locks that you manually acquire and release using an API.
* **Example in Java**: `ReentrantLock`.
* **How it works**:

  * Call `lock()` to acquire and `unlock()` to release.
  * Supports reentrant locking, try-lock with timeout, and interruptible lock acquisition.
* **Pros**: More flexible and powerful.
* **Cons**: You **must manually unlock**, or you risk deadlocks.

```java
ReentrantLock lock = new ReentrantLock();
lock.lock();
try {
    // critical section
} finally {
    lock.unlock();
}
```

---

## **3. Traditional Lock (Mutex / Binary Semaphore)**

* **Definition**: Low-level synchronization primitive used to ensure mutual exclusion.
* **Examples**:

  * **C / POSIX threads**: `pthread_mutex_t`
  * **Windows**: `CRITICAL_SECTION` or `Mutex`
* **How it works**:

  * Only one thread can acquire the lock at a time.
  * Other threads wait until the lock is released.
  * Ownership rules usually apply: only the thread that locked it can unlock it.
* **Pros**: Simple, predictable, and works at a lower level.
* **Cons**: Less flexible than high-level locks in Java.

---

## **4. Read-Write Lock**

* **Definition**: Allows multiple threads to read simultaneously but exclusive access for writing.
* **Example in Java**: `ReentrantReadWriteLock`
* **How it works**:

  * Multiple threads can hold the **read lock** at the same time.
  * Only one thread can hold the **write lock**, and no readers can hold it simultaneously.
* **Pros**: Improves concurrency when reads dominate writes.

---

## **5. Spinlock**

* **Definition**: A lock where the thread **spins in a loop**, waiting for the lock to become available instead of sleeping.
* **Use case**: Very short critical sections; avoids context-switch overhead.
* **Cons**: Wastes CPU if held for long.

---

## **6. Semaphore**

* **Definition**: Counting lock that allows up to **N threads** to access a resource simultaneously.
* **Binary Semaphore**: Acts like a mutex (max 1 thread).
* **Example**:

  * Java: `Semaphore`
  * POSIX: `sem_t`

---

### **Summary Table**

| Lock Type           | Java Example             | Key Feature                             |
| ------------------- | ------------------------ | --------------------------------------- |
| Implicit / Monitor  | `synchronized`           | Auto-lock/unlock, simple                |
| Explicit Lock       | `ReentrantLock`          | Manual lock/unlock, reentrant, flexible |
| Traditional / Mutex | `pthread_mutex_t`        | Low-level, one owner thread             |
| Read-Write Lock     | `ReentrantReadWriteLock` | Multiple readers, exclusive writer      |
| Spinlock            | Custom / low-level       | Busy-wait, avoids context switch        |
| Semaphore           | `Semaphore`              | Allows N threads, counting lock         |

---


# Database Locking:
---

## **1. Pessimistic Locking**

* **Definition**: Assume conflicts **will happen**, so you **lock the resource before accessing it**.
* **Goal**: Prevent other threads from modifying the resource until you’re done.
* **Typical Use**: Databases, critical sections with high contention.
* **How it works**:

  * Thread A acquires a lock on a resource.
  * Thread B must **wait** until Thread A releases the lock.
* **Pros**:

  * Prevents conflicts completely.
  * Safe for critical updates.
* **Cons**:

  * Can cause **blocking** or **deadlocks** if overused.
  * Performance can degrade with high contention.

**Example in Java database code (pseudo):**

```java
// Using pessimistic lock in JPA
EntityManager em;
MyEntity e = em.find(MyEntity.class, id, LockModeType.PESSIMISTIC_WRITE);
e.setValue("new value");
em.persist(e);
```

Here, **no other transaction can modify `e`** until this transaction commits.

---

## **2. Optimistic Locking**

* **Definition**: Assume conflicts **are rare**, so you **don’t lock upfront**; you check for conflicts when committing changes.
* **Goal**: Minimize blocking and improve performance when contention is low.
* **Typical Use**: Databases, distributed systems.
* **How it works**:

  * Read the resource.
  * Make your changes **without locking**.
  * When saving, check if the resource has changed since you read it (usually via a **version number** or **timestamp**).
  * If a conflict is detected, the operation **fails or retries**.
* **Pros**:

  * High concurrency, better performance with low conflicts.
  * No blocking.
* **Cons**:

  * Must handle conflicts explicitly.
  * May need retries if many conflicts occur.

**Example in Java database code (pseudo):**

```java
// Using optimistic lock in JPA
@Entity
class MyEntity {
    @Version
    private int version;  // incremented automatically
}

MyEntity e = em.find(MyEntity.class, id);
e.setValue("new value");
em.persist(e);  // Will throw OptimisticLockException if version changed
```

---

### **Key Difference: Pessimistic vs Optimistic**

| Feature           | Pessimistic Lock             | Optimistic Lock              |
| ----------------- | ---------------------------- | ---------------------------- |
| Assumption        | Conflicts **will happen**    | Conflicts **rarely happen**  |
| Locking           | Locks resource upfront       | No upfront lock              |
| Blocking          | Yes, others wait             | No blocking                  |
| Conflict Handling | Prevented by lock            | Detected at commit           |
| Best Use Case     | High contention              | Low contention               |
| Performance       | Can be slower due to waiting | Faster if conflicts are rare |

---

In short:

* **Pessimistic** = “I’ll lock it now, just in case someone else touches it.”
* **Optimistic** = “I’ll go ahead and hope nobody else changes it; if they do, I’ll handle it.”

---
# SUMMARY

| Lock Type           | Java Example             | Key Feature                             |
| ------------------- | ------------------------ | --------------------------------------- |
| Implicit / Monitor  | `synchronized`           | Auto-lock/unlock, simple                |
| Explicit Lock       | `ReentrantLock`          | Manual lock/unlock, reentrant, flexible |
| Traditional / Mutex | `pthread_mutex_t`        | Low-level, one owner thread             |
| Read-Write Lock     | `ReentrantReadWriteLock` | Multiple readers, exclusive writer      |
| Spinlock            | Custom / low-level       | Busy-wait, avoids context switch        |
| Semaphore           | `Semaphore`              | Allows N threads, counting lock , Example db pool        |

