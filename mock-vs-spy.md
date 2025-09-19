Perfect 👍 this is one of the most confusing areas in Mockito — the difference between a **Mock** and a **Spy**, especially when dealing with `void` methods. Let’s unpack it clearly.

---

## 🔹 Difference: `@Mock` vs `@Spy`

| Aspect         | `@Mock`                                                                                                         | `@Spy`                                                                                  |
| -------------- | --------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Origin         | Mockito (`org.mockito.Mock`)                                                                                    | Mockito (`org.mockito.Spy`)                                                             |
| Behavior       | By default, all methods return *default values* (`null`, `0`, `false`, empty collections). Nothing is executed. | By default, calls the **real methods** unless you stub them.                            |
| Use Case       | Use when you only care about interactions/outputs and don’t want real logic executed.                           | Use when you want a *partial mock*: mostly real behavior, but override/stub some parts. |
| Example Return | `mockedList.size()` → `0` (default, not real impl).                                                             | `spyList.size()` → actually calls real list method and returns real size.               |

---

## 🔹 Handling `void` methods

In Mockito, stubbing `void` methods is special. You can’t use `when(...).thenReturn(...)` because `void` doesn’t return anything. Instead, you use:

* `doNothing()` (default, but explicit is better)
* `doThrow()` (to simulate exceptions)
* `doAnswer()` (to implement custom behavior)

---

### ✅ Example with `@Mock`

```java
class NotificationService {
    void send(String message) {
        // imagine it sends a real email/SMS
        System.out.println("Sending: " + message);
    }
}
```

Test with `@Mock`:

```java
@ExtendWith(MockitoExtension.class)
class NotificationServiceTest {

    @Mock
    private NotificationService notificationService;

    @Test
    void testVoidWithMock() {
        // By default, does nothing, but we can be explicit
        doNothing().when(notificationService).send(anyString());

        // Or simulate exception
        doThrow(new RuntimeException("Failed")).when(notificationService).send("error");

        notificationService.send("hello"); // does nothing
        assertThrows(RuntimeException.class, () -> notificationService.send("error"));
    }
}
```

➡️ Here, `send()` never calls the real method.

---

### ✅ Example with `@Spy`

```java
@ExtendWith(MockitoExtension.class)
class NotificationServiceSpyTest {

    @Spy
    private NotificationService notificationService; // real object

    @Test
    void testVoidWithSpy() {
        // Without stubbing, real method runs
        notificationService.send("hello"); 
        // prints "Sending: hello"

        // Override behavior for a specific case
        doNothing().when(notificationService).send("silent");
        notificationService.send("silent"); // does nothing

        // Simulate exception
        doThrow(new RuntimeException("Boom")).when(notificationService).send("fail");
        assertThrows(RuntimeException.class, () -> notificationService.send("fail"));
    }
}
```

➡️ By default, `Spy` calls the **real method** unless overridden.

---

## 🔑 Key Takeaways

* **Mock** → Fake object, all methods mocked unless you stub them.
* **Spy** → Wraps a real object, so real methods run unless you stub them.
* For **void methods**, use `doNothing()`, `doThrow()`, or `doAnswer()`.

---

Perfect 👍 let’s build an example with a **`void` method** called `sum(int a, int b)` that just prints or logs the result. Since it’s `void`, we can’t capture its return, but we can use `doAnswer` to **intercept the call, inspect arguments, and simulate behavior**.

---

### 🔹 Example Class Under Test

```java
public class CalculatorService {
    public void sum(int a, int b) {
        // In real scenario it might write to DB, log, send a message etc.
        System.out.println("Result: " + (a + b));
    }
}
```

---

### 🔹 Test with `doAnswer` on a **Mock**

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.Mockito.doAnswer;

@ExtendWith(MockitoExtension.class)
class CalculatorServiceMockTest {

    @Mock
    private CalculatorService calculatorService;

    @Test
    void testSumWithDoAnswerOnMock() {
        doAnswer(invocation -> {
            int a = invocation.getArgument(0);
            int b = invocation.getArgument(1);
            int result = a + b;
            System.out.println("Intercepted sum: " + result);
            return null; // void method → return null
        }).when(calculatorService).sum(anyInt(), anyInt());

        calculatorService.sum(5, 7); // prints "Intercepted sum: 12"
    }
}
```

➡️ Here, the real `sum` is **never executed** (because it’s a mock), but we **simulate behavior** inside `doAnswer`.

---

### 🔹 Test with `doAnswer` on a **Spy**

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Spy;
import org.mockito.junit.jupiter.MockitoExtension;

import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.Mockito.doAnswer;

@ExtendWith(MockitoExtension.class)
class CalculatorServiceSpyTest {

    @Spy
    private CalculatorService calculatorService; // real object

    @Test
    void testSumWithDoAnswerOnSpy() {
        doAnswer(invocation -> {
            int a = invocation.getArgument(0);
            int b = invocation.getArgument(1);
            int result = a + b;
            System.out.println("Custom spy behavior: " + result * 2);
            return null;
        }).when(calculatorService).sum(anyInt(), anyInt());

        calculatorService.sum(3, 4); 
        // Instead of real method, prints "Custom spy behavior: 14"
    }
}
```

➡️ Normally, a `Spy` would run the real `sum`, but because of `doAnswer`, we override it with custom behavior.

---

## 🔑 Takeaway

* **`doAnswer`** is super flexible:

  * You can inspect arguments (`invocation.getArgument(i)`).
  * You can perform custom logic.
  * You can simulate return values or side effects (logging, callback, etc.).
* For **void methods**, `doAnswer` is often the best choice when you need more than just `doNothing()` or `doThrow()`.

---
