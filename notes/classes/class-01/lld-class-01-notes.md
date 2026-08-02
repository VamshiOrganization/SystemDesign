## LLD Notes 01 - Why Software Design Exists Index [PDF](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/class-01/lld-class-01-notes.pdf)

### 1. The Big Picture

-   **Programming vs. Software Engineering** _(Writing vs. Changing Code)_ — Section 1
    
-   **The Structural Handicap of the Software Industry** — Section 1
    
-   **Design in the AI Era & Token Costs of Coupling** — Section 1
    
-   **The True Cost of Rushing Code** — Section 1
    

### 2. Symptoms of Bad Code

-   **Rigidity** _(Cascade of Forced Changes)_ — Section 2.1
    
-   **Fragility** _(Unrelated Breakages & Unmaintainable Code)_ — Section 2.2
    
-   **Immobility** _(Inability to Reuse & Welded Code)_ — Section 2.3
    
-   **The Root Cause:** Coupling & Dependency Directions — Section 2.4
    

### 3. Coupling and Cohesion

-   **Coupling Spectrum (Worst to Best):** Content, Common, Control, Stamp, Data, and Message Coupling — Section 3.1
    
-   **Tight vs. Loose Coupling Examples** _(Java & Python)_ — Section 3.1
    
-   **Cohesion:** Focused Purpose vs. "Utils" Dumping Ground — Section 3.2
    
-   **Interplay Between Coupling and Cohesion** _(Quick Self-Test)_ — Section 3.3
    

### 4. Flow of Control vs. Source Code Dependency

-   **The Two Arrows:** Runtime Control Flow vs. Compile-Time Dependencies — Section 4.1
    
-   **The Universal Law of Procedural Code** — Section 4.1
    
-   **The Procedural Call Tree & Upward Propagation of Details** — Section 4.2
    
-   **Real-World Impact:** Checkout $\rightarrow$ Stripe Dependency Chain — Section 4.3
    

### 5. The Three "Magic Words" of OO — An Honest Audit

-   **Encapsulation:** C vs. C++ Header Isolation Audit — Section 5.1
    
-   **Inheritance:** Diamond Problem, MRO, and Abstract Classes vs. Interfaces — Section 5.2
    
-   **Polymorphism (The Real Prize):**
    
    -   UNIX `copy` Program & Function Pointer Tables (vtable) — Section 5.3
        
    -   Static Typing vs. Dynamic Duck Typing — Section 5.3
        
-   **The Audit Scorecard & True Purpose of OO** — Section 5.4
    

### 6. Inversion of Control (IoC) — The Real Reason OO Exists

-   **Reversing Source Code Dependencies via Abstractions** — Section 6.1 & 6.2
    
-   **Hollywood Principle:** Control Shift from Details to Policy — Section 6.3
    
-   **Worked Example 1:** Checkout $\rightarrow$ Stripe Inverted _(4 Key Benefits)_ — Section 6.4
    
-   **Worked Example 2:** Order Notifications Architecture — Section 6.5
    
-   **Untangling Concepts:** IoC vs. DIP vs. DI — Section 6.6
    
-   **Where Dependency Inversion Appears** — Section 6.7
    
-   **Core Definition to Memorize** — Section 6.8
    

### 7. The SOLID Principles

-   **S — Single Responsibility Principle (SRP):**
    
    -   Definition & Actor-Based Changes _(Employee Class Trap)_ — Section 7.1
        
    -   Code Refactoring Examples _(Java & Python)_ — Section 7.1
        
-   **O — Open-Closed Principle (OCP):**
    
    -   The Switch-Statement Disease & Violation Costs — Section 7.2
        
    -   Compliant Abstractions & The Honest Caveat on Prediction — Section 7.2
        
-   **L — Liskov Substitution Principle (LSP):**
    
    -   Rectangle/Square & Bird/Penguin Violations — Section 7.3
        
    -   Preconditions, Postconditions, and Substitutability — Section 7.3
        
-   **I — Interface Segregation Principle (ISP):**
    
    -   Fat Interfaces vs. Role-Based Capabilities — Section 7.4
        
    -   Spotting Violations in the Wild — Section 7.4
        
-   **D — Dependency Inversion Principle (DIP):**
    
    -   Package Ownership of Abstractions _(Ports & Adapters)_ — Section 7.5
        
    -   Weather App Before/After Refactoring — Section 7.5
        
    -   Composition Roots & Identifying DIP Violations — Section 7.5
        

### 8. Summary

-   **Key Takeaways & The Developer's Role in the AI Era** — Section 8
