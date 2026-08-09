[<~ Back to LLD](../lld.md)

### Design Patterns, Builder, Strategy, and Null Index

### ** Video (08-08-2026) ** **File Reference:** [class-03-notes.pdf](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/lld/class-03/class-03-notes.pdf) (Total Pages: 8)
 
### **1. What Design Patterns Are (and Aren't)** — Page 1

-   **Origin & Gang of Four (GoF):** Cataloguing 23 recurring OOP solutions and creating a common vocabulary.
    
-   **Misconceptions:** Design patterns are not checklists, measures of seniority, or universally required.
    
-   **Core Mental Model:** Focus on code readability, maintainability, and extendability; allow patterns to be discovered naturally rather than forced.
    

### **2. Builder Pattern** — Pages 1–3

-   **2.1 The Problem (Telescoping Constructors):** Handling $2^n$ combinations of optional fields, unreadable invocation sites, and positional argument bugs.
    
-   **2.2 The Solution (Fluent Builder):** Step-by-step mutable helper, centralized defaults, validation on `.build()`, and immutable object output.
    
-   **2.3 Language Evolution Impact:** How named parameters in Python, Kotlin, Swift, and C# resolve the telescoping issue without requiring extra builder classes.
    

### **3. Strategy Pattern** — Pages 3–5

-   **3.1 Problem Definition:** Evolving business logic forcing code duplication across similar loops (separating stable loops from volatile decisions).
    
-   **3.2 Core Concept:** Decoupling decisions by passing behavior as parameters (Open-Closed Principle).
    
-   **3.3 Heavyweight Implementation:** Pre-Java 8 ceremony using custom interfaces and standalone concrete classes.
    
-   **3.4 Lightweight Implementation:** Java 8 lambdas (`@FunctionalInterface`), method references, and Stream API integration.
    
-   **3.5 Real-World Example (Org Tree Appraisals):** Recursive employee tree traversal paired with pluggable appraisal formulas (mean, median, min-max).
    

### **4. Null — "The Billion-Dollar Mistake"** — Pages 5–6

-   **4.1 Why Null is a Smell:** Runtime exceptions distant from origin, absence hidden in signatures, and check-pollution.
    
-   **4.2 Java's Solution (`Optional<T>`):** Representing absence in method signatures and enforcing caller decision handling.
    
-   **4.3 Anti-Patterns & Best Practices:** Ground rules for avoiding returned nulls, handling empty collections, avoiding bare `.get()` calls, and cleansing boundary inputs.
    
-   **4.4 Type-System Protections:** How Kotlin, Swift, and C# enforce non-nullable vs. nullable types at compile time.
    

### **5. Summary** — Page 6

-   Key takeaways summarizing pattern vocabulary, Builder advantages, Strategy mechanics, appraisal tree execution, and null handling rules.
    

### **6. Appendix — The Pattern Catalogue** — Pages 6–8

-   **Creational Patterns (5):** Abstract Factory, Builder, Factory Method, Prototype, Singleton.
    
-   **Structural Patterns (7):** Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy.
    
-   **Behavioral Patterns (11):** Chain of Responsibility, Command, Interpreter, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor.
    
-   **Beyond the Gang of Four:** Dependency Injection, Null Object, Object Pool, Repository, Immutable Object, Composition over Inheritance, Fluent Interface/Pipeline, Lazy Evaluation, and Futures/Async.