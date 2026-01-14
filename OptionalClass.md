### Java Optional Interview Questions and Answers

---

#### 1. What is `Optional` in Java? Why was it introduced?

**Answer**: `Optional` is a container object used to contain not-null objects. It was introduced in Java 8 to reduce the risk of `NullPointerException` and provide a more functional approach to handling null values.

#### 2. How do you create an `Optional` object?

**Answer**:

```java
Optional<String> opt1 = Optional.of("Hello");
Optional<String> opt2 = Optional.ofNullable(null);
Optional<String> opt3 = Optional.empty();
```

#### 3. What is the difference between `Optional.of()` and `Optional.ofNullable()`?

**Answer**: `Optional.of()` throws `NullPointerException` if the value is null, whereas `Optional.ofNullable()` returns an empty `Optional` if the value is null.

#### 4. What happens if you pass `null` to `Optional.of()`?

**Answer**: It throws a `NullPointerException`.

#### 5. How do you retrieve a value from an `Optional`?

**Answer**: Using `get()`, but it should be avoided if the presence of a value is not guaranteed.

```java
Optional<String> opt = Optional.of("Hello");
String value = opt.get();
```

#### 6. What is the purpose of `isPresent()` and `get()`?

**Answer**: `isPresent()` checks if a value is present. `get()` retrieves the value but should be used only after checking `isPresent()`.

#### 7. Why is using `get()` discouraged?

**Answer**: Because if the `Optional` is empty, it throws `NoSuchElementException`. Prefer `orElse()`, `orElseGet()`, or `orElseThrow()`.

#### 8. What are the benefits of using `Optional` over null checks?

**Answer**: It makes the code more readable, reduces boilerplate null checks, and encourages better API design.

#### 9. How do `ifPresent()` and `ifPresentOrElse()` work?

**Answer**:

```java
opt.ifPresent(System.out::println);
opt.ifPresentOrElse(System.out::println, () -> System.out.println("Not present"));
```

#### 10. How do you provide a default value using `orElse()` and `orElseGet()`?

**Answer**:

```java
String result = opt.orElse("default");
String result = opt.orElseGet(() -> "default");
```

#### 11. What’s the difference between `orElse()` and `orElseGet()`?

**Answer**: `orElse()` always evaluates its argument, even if the value is present. `orElseGet()` evaluates its supplier only if the value is absent.
orElse() — always evaluates its argument and orElseGet(), even the optional has the value in it v/s evaluates only if needed when the optional does not have the value
#### 12. How do you throw an exception if a value is absent?

**Answer**:

```java
opt.orElseThrow(() -> new IllegalArgumentException("Value not found"));
```

#### 13. How can you transform the value inside an `Optional`?

**Answer**: Using `map()`.

```java
Optional<Integer> length = opt.map(String::length);
```

#### 14. How is `flatMap()` different from `map()` in the context of `Optional`?

**Answer**: `map()` returns a nested `Optional`, while `flatMap()` flattens it.

```java
Optional<String> upper = opt.flatMap(val -> Optional.of(val.toUpperCase()));
```

#### 15. How do you filter a value inside an `Optional`?

**Answer**:

```java
opt.filter(val -> val.length() > 3);
```

#### 16. Can `Optional` be used as a method parameter or class field? Why or why not?

**Answer**: It's discouraged because it can lead to confusion and unnecessary complexity. It's mainly intended for return types.

#### 17. What are some anti-patterns or bad practices when using `Optional`?

**Answer**: Using `Optional` as method parameters, fields, or in collections.

#### 18. Can you use `Optional` with collections? Why or why not?

**Answer**: No, using `Optional` inside collections is discouraged due to readability and unnecessary nesting.

#### 19. How would you chain multiple Optional operations safely?

**Answer**:

```java
opt.map(User::getAddress)
   .map(Address::getCity)
   .orElse("Unknown");
```

#### 20. How can you convert a value from `Optional<T>` to `Stream<T>`?

**Answer**:

```java
opt.stream(); // Returns Stream<T>
```

#### 21. How is `Optional` different from `OptionalInt`, `OptionalDouble`, etc.?

**Answer**: Primitive `Optional` classes avoid boxing overhead for primitives.

#### 22. Is `Optional` serializable? What problems can arise from that?

**Answer**: It is not guaranteed to be serializable. Using it in entities or models can cause issues.

#### 23. How does `Optional` help in writing cleaner APIs?

**Answer**: It clearly communicates that a method might return a missing value and avoids null checks.

#### 24. Write a method that returns an `Optional<User>` if a user exists.

**Answer**:

```java
public Optional<User> findUserById(String id) {
    return users.stream()
                .filter(user -> user.getId().equals(id))
                .findFirst();
}
```

#### 25. Refactor a method using null to use `Optional`.

**Before**:

```java
User findUser(String id) {
    if (id == null) return null;
    return ...;
}
```

**After**:

```java
Optional<User> findUser(String id) {
    if (id == null) return Optional.empty();
    return Optional.of(...);
}
```

#### 26. Use `Optional` to avoid `NullPointerException` in nested calls.

**Answer**:

```java
String city = Optional.ofNullable(user)
                      .map(User::getAddress)
                      .map(Address::getCity)
                      .orElse("Unknown");
```

#### 27. Demonstrate `Optional` with `Stream` API.

**Answer**:

```java
list.stream()
    .map(this::findUser)
    .flatMap(Optional::stream)
    .collect(Collectors.toList());
```

#### 28. Compare using `Optional` vs `null`.

**Answer**: `Optional` is more expressive and prevents null-related bugs. Null is implicit and error-prone.

#### 29. Real-world use case: REST API response parsing.

**Answer**: When parsing optional fields in a JSON response, use `Optional` to represent absence cleanly.

#### 30. Simulate a real-world scenario using `Optional`.

**Example**:

```java
public Optional<Product> findProduct(String sku) {
    return productRepo.stream()
                      .filter(p -> p.getSku().equals(sku))
                      .findFirst();
}
```
