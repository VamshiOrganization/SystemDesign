[<~ Back to LLD](../lld.md)

## Low-Level Design (LLD): Singleton, Functional Java, Streams, and Modern Decorator

### **Video Date: (08-09-2026)** and **File Reference:** [class-04-notes.pdf](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/lld/class-04/class-04-notes.pdf) and  [lld-practice-problems-set2](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/lld/class-04/lld-practice-problems-set2) 
 


### **1. Singleton Pattern**

-   **1.1 The Problem:** Ensuring an object exists exactly once per application (e.g., loggers, config holders, connection pools).
    
-   **1.2 Implementation Approaches:**
    
    -   **Attempt 1 (Eager Initialization):** Thread-safe via JVM class loading, but instantiates regardless of usage.
        
    -   **Attempt 2 (Lazy Initialization - Broken):** Non-atomic check-then-act (`INSTANCE == null`) creates race conditions across threads.
        
    -   **Attempt 3 (Holder Idiom):** Lazy and thread-safe without locks; relies on the JVM loading inner static classes on first access.
        
    -   **Attempt 4 (Enum Singleton):** Joshua Bloch's recommended single-line approach; inherently thread-safe, handles serialization, and prevents reflection attacks.
        
-   **1.3 Architectural Caveats:** Singleton as global mutable state; transition toward Dependency Injection (DI) and composition root wiring (e.g., Spring singleton scope).
    

### **2. Functional Interfaces & Functions as Values**

-   **2.1 Core Concept:** Interfaces with exactly one abstract method enable lambdas as parameter values.
    
-   **2.2 The Big Four (`java.util.function`):**
    
    -   **`Function<T, R>`:** Transform input $T$ to output $R$ via `.apply()`.
        
    -   **`Predicate<T>`:** Evaluate condition on $T$ returning `boolean` via `.test()`.
        
    -   **`Consumer<T>`:** Perform action on $T$ with no return value via `.accept()`.
        
    -   **`Supplier<R>`:** Produce value $R$ without input via `.get()`.
        
-   **2.3 Functional Relatives & Composition:** `BiFunction`, `UnaryOperator`, `BinaryOperator`, `Runnable`, plus chaining methods (`andThen`, `and`, `or`, `negate`).
    
-   **2.4 Method References (`::`):** Syntax shorthand for forwarding lambdas across four kinds (Static, Instance on specific object, Instance on arbitrary object, Constructor).
    

### **3. Java Streams Crash Course**

-   **3.1 Pipeline Structure:** Source $\rightarrow$ Intermediate Operations (lazy) $\rightarrow$ Terminal Operation (eager trigger).
    
-   **3.2 Execution Fusing & Laziness:** Dispelling the "multiple loops" myth—stream pipelines fuse operations into a single element-at-a-time pass with zero intermediate collection allocations.
    
-   **3.3 Short-Circuiting & Parallelism:** Pulling elements on demand enables infinite streams (`Stream.iterate`); `.parallel()` enables multi-core processing when using pure lambdas.
    
-   **3.4 Pure Functions:** Functions without side effects (output depends strictly on inputs) as a prerequisite for thread safety, predictable composition, and parallel operations.
    

### **4. Decorator Pattern — Classic vs. Modern Functional Approach**

-   **4.1 The Classic Wrapper Approach:** Implementing shared interfaces and wrapping instances to avoid class explosions ($2^n$ subclasses).
    
-   **4.2 The Modern Functional Approach:** Replacing whole wrapper classes with single `Function<String, String>` transformations chained via `.andThen()`.
    
-   **4.3 The Identity Function & Reduction:** Utilizing `Function.identity()` as the seed value in `Stream.reduce` to fold arbitrary lists of decorators into a single pipeline.
    
-   **4.4 `TextProcessor` Example:** Implementing flexible, open-closed text formatting through functional composition.
    

### **5. Summary: GoF Patterns Collapsing into Language Features**

-   **Strategy:** Replaced by Lambdas.
    
-   **Decorator:** Replaced by `Function.andThen()` composition.
    
-   **Iterator:** Replaced by Streams and `for-each` loops.
    
-   **Command:** Replaced by `Consumer` / `Runnable` execution blocks.
    
-   **Template Method:** Replaced by passing high-order `Function` parameters.
    
-   **Singleton:** Replaced by Enums or DI container scope management.