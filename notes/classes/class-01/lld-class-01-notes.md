## LLD Notes 01 - Why Software Design Exists Index [PDF](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/class-01/lld-class-01-notes.pdf)


### 1. The Big Picture

-   **Programming vs. Software Engineering** _(Writing 1,000 Lines vs. Changing 1 Line)_ — Page 1
    
-   **The Structural Handicap of the Software Industry** — Page 2
    
-   **Design in the AI Era & Token Costs of Coupling** — Page 3
    
-   **The True Cost of Rushing Code & Developer Responsibility** — Page 3
    

### 2. Symptoms of Bad Code

-   **Rigidity** _(Cascade of Forced Changes)_ — Page 4
    
-   **Fragility** _(Unrelated Breakages & Unmaintainable Code)_ — Page 5
    
-   **Immobility** _(Inability to Reuse & Welded Code)_ — Page 5
    
-   **The Root Cause:** Coupling & Dependency Directions — Page 6
    

### 3. Coupling and Cohesion

-   **Coupling Spectrum (Worst to Best):** Content, Common, Control, Stamp, Data, and Message Coupling — Pages 7–8
    
-   **Tight vs. Loose Coupling Examples** _(Java & Python)_ — Pages 8–9
    
-   **Cohesion:** Focused Purpose vs. "Utils" Dumping Ground — Pages 9–10
    
-   **Interplay Between Coupling and Cohesion** _(Quick Self-Test)_ — Page 10
    

### 4. Flow of Control vs. Source Code Dependency

-   **The Two Arrows:** Runtime Control Flow vs. Compile-Time Dependencies — Page 11
    
-   **The Universal Law of Procedural Code** — Page 12
    
-   **The Procedural Call Tree & Upward Propagation of Details** — Page 12
    
-   **Real-World Impact:** Checkout $\rightarrow$ Stripe Dependency Chain — Page 13
    

### 5. The Three "Magic Words" of OO — An Honest Audit

-   **Encapsulation:** C vs. C++ Header Isolation Audit — Page 14
    
-   **Inheritance:** Diamond Problem, MRO, and Abstract Classes vs. Interfaces — Page 15
    
-   **Polymorphism (The Real Prize):**
    
    -   UNIX `copy` Program & Function Pointer Tables (vtable) — Page 16
        
    -   Static Typing vs. Dynamic Duck Typing — Page 16
        
-   **The Audit Scorecard & True Purpose of OO** — Page 17
    

### 6. Inversion of Control (IoC) — The Real Reason OO Exists

-   **Reversing Source Code Dependencies via Abstractions** — Page 17
    
-   **Hollywood Principle:** Control Shift from Details to Policy — Page 18
    
-   **Worked Examples:** Checkout $\rightarrow$ Stripe Inverted & Order Notifications — Page 18
    

### 7. The SOLID Principles

-   **S — Single Responsibility Principle (SRP):**
    
    -   Definition & Actor-Based Changes _(Employee Class Trap)_ — Pages 19–20
        
    -   Code Refactoring Examples _(Java & Python)_ — Pages 20–21
        
-   **O — Open-Closed Principle (OCP):**
    
    -   The Switch-Statement Disease & Violation Costs — Pages 21–22
        
    -   Compliant Abstractions & The Honest Caveat on Prediction — Pages 22–23
        
-   **L — Liskov Substitution Principle (LSP):**
    
    -   Rectangle/Square & Bird/Penguin Violations — Pages 23–24
        
    -   Preconditions, Postconditions, and Substitutability — Pages 24–25
        
-   **I — Interface Segregation Principle (ISP):**
    
    -   Fat Interfaces vs. Role-Based Capabilities — Pages 25–26
        
    -   Spotting Violations in the Wild — Page 27
        
-   **D — Dependency Inversion Principle (DIP):**
    
    -   Package Ownership of Abstractions _(Ports & Adapters)_ — Pages 27–28
        
    -   Weather App Before/After Refactoring — Page 28
        
    -   Composition Roots & Identifying DIP Violations — Page 29
        

### 8. Summary

-   **8 Core Takeaways & Design Rules in the AI Era** — Pages 30–31
