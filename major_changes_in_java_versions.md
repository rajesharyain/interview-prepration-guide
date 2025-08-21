# Summary:
Java 8 was a watershed—introducing lambdas, streams, and modern date-time APIs. Java 11 brought a standard HttpClient and modernized GC with ZGC. Java 17 LTS added sealed classes, switch pattern matching, and Foreign-Memory APIs. Java 21 LTS embraced virtual threads and structured concurrency for simpler multithreading. Java 23, although non-LTS, further improves productivity with features like Markdown JavaDoc, flexible constructors, primitive pattern matching, and the class-file API.”

##
| Version | Release (LTS?) | Key Features                                                                |
| ------- | -------------- | --------------------------------------------------------------------------- |
| **8**   | 2014 (LTS)     | Lambdas, Streams, Default methods, `java.time`                              |
| **11**  | 2018 (LTS)     | HttpClient, ZGC, removal of Applets, JavaFX, improved TLS, PKCS11 updates   |
| **14**  | 2020 (Non-LTS) | Records (preview), pattern-match `instanceof`, text blocks                  |
| **17**  | 2021 (LTS)     | Sealed classes, switch pattern matching, Vector & Foreign APIs, performance |
| **21**  | 2023 (LTS)     | Virtual threads, structured concurrency, scoped values, unnamed classes     |
| **23**  | 2024 (Non-LTS) | Doc improvements, primitive patterns, flexible constructors, class-file API |






### Major Changes from Java 8 to Java 17

#### Java 9
1. **Modular System (Project Jigsaw)**:
   - Introduced modules to improve modularity and encapsulation. 
   - Allows developers to define module dependencies and expose only specific parts of a module.

2. **JShell**:
   - A new Read-Eval-Print Loop (REPL) tool for interactive Java programming.

3. **Enhanced `@Deprecated` Annotation**:
   - New elements like `since` and `forRemoval` to better document the deprecation status.

4. **Improved Stream API**:
   - Added `takeWhile`, `dropWhile`, and `ofNullable` methods to the Stream API.

5. **New Collection Factory Methods**:
   - Methods like `List.of`, `Set.of`, and `Map.of` for easy and concise collection creation.

#### Java 10
1. **Local Variable Type Inference**:
   - Introduction of the `var` keyword to simplify local variable declarations.

2. **Garbage Collector Improvements**:
   - Introduction of the G1 garbage collector improvements and experimental support for the ZGC (Garbage Collector).

3. **Application Class-Data Sharing**:
   - Enhancement to improve startup performance by sharing class metadata between Java processes.

#### Java 11
1. **Long-Term Support (LTS)**:
   - Java 11 is an LTS release, meaning it will receive extended support and updates.

2. **String Methods**:
   - New methods like `isBlank`, `lines`, `strip`, `stripLeading`, and `stripTrailing` for easier string manipulation.

3. **Local-Variable Syntax for Lambda Parameters**:
   - Allows the use of `var` in lambda expressions.

4. **HTTP Client API**:
   - Standardized API for HTTP/2 and WebSocket support.

5. **Removed Java EE and CORBA Modules**:
   - Removed deprecated Java EE and CORBA modules from the JDK.

#### Java 12
1. **JVM Constants API**:
   - Introduced the `java.lang.invoke` package for better JVM constants handling.

2. **Switch Expressions (Preview)**:
   - Enhanced switch statements to allow expressions and return values.

3. **Shenandoah Garbage Collector**:
   - New low-latency garbage collector.

#### Java 13
1. **Text Blocks (Preview)**:
   - Simplified syntax for multi-line string literals with `"""`.

2. **Dynamic CDS Archives**:
   - Enhancements to Class Data Sharing (CDS) to allow dynamic archiving of classes.

3. **ZGC (Experimental)**:
   - Further enhancements and experimental status for Z Garbage Collector.

#### Java 14
1. **Records (Preview)**:
   - Introduction of record classes for immutable data carriers.

2. **Pattern Matching for `instanceof` (Preview)**:
   - Simplified type checks and casting.

3. **NullPointerException Improvements**:
   - Enhanced messages for `NullPointerException` to make debugging easier.

4. **Foreign-Memory Access API (Incubator)**:
   - Introduced to interact with memory outside the Java heap.

#### Java 15
1. **Text Blocks (Standard)**:
   - Text blocks become a standard feature.

2. **Sealed Classes (Preview)**:
   - Classes that can restrict which other classes or interfaces may extend or implement them.

3. **Hidden Classes**:
   - Added support for classes that are not accessible by the application but used by frameworks and libraries.

#### Java 16
1. **Records (Standard)**:
   - Records are standardized, providing a concise way to define data-carrying classes.

2. **Pattern Matching for `instanceof` (Standard)**:
   - Pattern matching for `instanceof` becomes a standard feature.

3. **JEP 394: Switch Expressions (Standard)**:
   - Switch expressions become standard, simplifying switch statements and adding new functionalities.

4. **JEP 395: ZGC (Standard)**:
   - Z Garbage Collector becomes a standard feature.

#### Java 17
1. **Long-Term Support (LTS)**:
   - Java 17 is an LTS release, providing long-term support and stability.

2. **Sealed Classes (Standard)**:
   - Sealed classes are standardized, allowing control over class inheritance.

3. **Pattern Matching for `switch` (Preview)**:
   - Enhanced `switch` statements with pattern matching capabilities.

4. **Foreign Function & Memory API (Incubator)**:
   - New API to interact with code and data outside the JVM.

5. **JEP 391: macOS/AArch64 Port**:
   - Support for macOS on ARM64 architecture.

### Major Changes from Java 17 to Java 21

#### Java 18
1. **Simple Web Server**:
   - A minimal web server for development and testing purposes.

2. **UTF-8 by Default**:
   - The default character encoding for the JVM is set to UTF-8.

3. **Code Snippets in Java API Documentation**:
   - Support for embedding code snippets in API documentation.

#### Java 19
1. **Record Patterns (Preview)**:
   - Enhanced pattern matching for records.

2. **Pattern Matching for `switch` (Second Preview)**:
   - Further improvements and refinements in pattern matching for `switch`.

3. **Virtual Threads (Preview)**:
   - Introduction of virtual threads to simplify concurrency.

4. **Foreign Function & Memory API (Second Incubator)**:
   - Further enhancements to the Foreign Function & Memory API.

#### Java 20
1. **Record Patterns (Standard)**:
   - Record patterns become a standard feature.

2. **Pattern Matching for `switch` (Standard)**:
   - Pattern matching for `switch` becomes a standard feature.

3. **Virtual Threads (Second Preview)**:
   - Further enhancements to virtual threads.

4. **Scoped Values (Incubator)**:
   - Introduced to provide a way to manage thread-local values.

#### Java 21
1. **Long-Term Support (LTS)**:
   - Java 21 is an LTS release, ensuring long-term support and stability.

2. **Pattern Matching for Switch (Standard)**:
   - Standardized pattern matching for `switch`.

3. **Project Loom**:
   - Full support for virtual threads, making concurrency simpler and more scalable.

4. **Project Panama**:
   - Standardized support for the Foreign Function & Memory API.

5. **Project Amber**:
   - Continued improvements to language features, including pattern matching and record classes.
