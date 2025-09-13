### **The Contract (from `Object` class docs)**

1. **If two objects are equal according to `equals()`**, they **must** have the same `hashCode()`.

   * Formally:

     ```java
     if a.equals(b) == true  -->  a.hashCode() == b.hashCode()
     ```
2. **If two objects have the same `hashCode()`, they do NOT have to be equal according to `equals()`**.

---

### **Why overriding `equals()` requires overriding `hashCode()`**

* Collections like `HashSet`, `HashMap` use **hashCode() first** to find the bucket where an object might be stored.
* If you only override `equals()` and not `hashCode()`:

  * Two “equal” objects (per `equals()`) may have **different hash codes**.
  * Hash-based collections look in **different buckets**, so they **never call `equals()`**.
  * Result: your collection won’t behave correctly (like in your `Celebrity` example).

**Analogy:**
Think of `hashCode()` as the “room number” in a hotel and `equals()` as checking the “name on the reservation.”

* Overriding `equals()` but not `hashCode()` is like saying two people have the same reservation name but are assigned to **different rooms**. If the receptionist only checks the room number first, they’ll never realize it’s the same person.

---

### **Why overriding `hashCode()` alone doesn’t require overriding `equals()`**

* Overriding `hashCode()` just changes which bucket an object goes into.
* If `equals()` is not overridden, equality is still **reference equality (`==`)**.
* HashSet will still check `equals()` (default `Object.equals`) if two objects land in the same bucket.
* So nothing “breaks the contract” in this case.

**Analogy:**
Giving two people the same room number (`hashCode()`) doesn’t make them the same person (`equals()`). That’s fine; it just means the receptionist checks the name next.

---

### **Summary Table**

| Case                                    | Works in HashSet? | Reason                                                     |
| --------------------------------------- | ----------------- | ---------------------------------------------------------- |
| Override `equals()` only                | ❌ No              | Hash codes differ → wrong bucket → `equals()` never called |
| Override `hashCode()` only              | ✅ Yes             | Equality still uses reference → contract not broken        |
| Override both `equals()` + `hashCode()` | ✅ Yes             | Works correctly                                            |

---

💡 **Key Rule:**

> **Equals implies hashCode**, but **hashCode alone does not imply equals**.



## **1️⃣ Default `equals()` in `Object`**

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

* **Behavior:**

  * Compares **references**, not content.
  * Returns `true` only if the two references point to **the exact same object in memory**.
* **Implication:**

  * By default, two objects with the same data but stored in different memory locations are **not equal**.

**Example:**

```java
Object a = new Object();
Object b = new Object();

System.out.println(a.equals(b)); // false
System.out.println(a == b);      // false
System.out.println(a.equals(a)); // true
```

✅ Default `equals()` = **reference equality**.

---

## **2️⃣ `String` class**

```java
@Override
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String) anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            for (int i = 0; i < n; i++) {
                if (v1[i] != v2[i])
                    return false;
            }
            return true;
        }
    }
    return false;
}
```

* **Behavior:**

  * Compares the **actual sequence of characters** inside the string.
  * Two different `String` objects with the same characters are considered equal.

**Example:**

```java
String s1 = new String("Hello");
String s2 = new String("Hello");

System.out.println(s1.equals(s2)); // true
System.out.println(s1 == s2);      // false
```

* `hashCode()` is also overridden in `String` to match `equals()` (based on character values).

---

## **3️⃣ `Integer` class**

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;
    if (obj instanceof Integer) {
        return value == ((Integer)obj).intValue();
    }
    return false;
}
```

* **Behavior:**

  * Compares the **int value** stored inside the `Integer` object.
  * Two different `Integer` objects with the same numeric value are equal.

**Example:**

```java
Integer i1 = new Integer(100);
Integer i2 = new Integer(100);

System.out.println(i1.equals(i2)); // true
System.out.println(i1 == i2);      // false
```

* `hashCode()` is overridden to return the **int value itself**.

---

## **4️⃣ Other common classes**

| Class       | Equals Behavior                             |
| ----------- | ------------------------------------------- |
| `Double`    | Compares numeric value (`doubleValue()`)    |
| `Character` | Compares `char` value                       |
| `ArrayList` | Compares elements in order using `equals()` |
| `HashMap`   | Compares key-value mappings                 |

* Most **immutable classes** (`String`, `Integer`, `Double`) override `equals()` to compare **contents**, not references.
* **Mutable classes** (like `Object`, `Point` if not overridden) often keep reference equality.

---

### ✅ **Summary**

* **Object:** `equals()` → reference equality (`==`)
* **String, Integer, Double, etc.:** `equals()` → **value/content equality**
* Always remember: if you override `equals()`, you must override `hashCode()` to maintain the contract in hash-based collections.


