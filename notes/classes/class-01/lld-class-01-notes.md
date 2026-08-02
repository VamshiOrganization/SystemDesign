## LLD Notes 01 - Why Software Design Exists Index [PDF](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/class-01/lld-class-01-notes.pdf)

### 1. The Big Picture

-   **Programming vs. Software Engineering** _(Writing 1,000 Lines vs. Changing 1 Line)_ — Pages 1–2
    
-   **The Structural Handicap of the Software Industry** — Page 3
    
-   **Design in the AI Era & Token Costs of Coupling** — Page 4
    
-   **The True Cost of Rushing Code & Developer Responsibility** — Page 5
    

### 2. Symptoms of Bad Code

-   **Rigidity** _(Cascade of Forced Changes)_ — Page 6
    
-   **Fragility** _(Unrelated Breakages & Unmaintainable Code)_ — Page 7
    
-   **Immobility** _(Inability to Reuse & Welded Code)_ — Page 8
    
-   **The Root Cause:** Coupling & Dependency Directions — Page 9
    

### 3. Coupling and Cohesion

-   **Coupling Spectrum (Worst to Best):** Content, Common, Control, Stamp, Data, and Message Coupling — Page 10
    
-   **Tight vs. Loose Coupling Examples** _(Java & Python)_ — Pages 11–12
    
-   **Cohesion:** Focused Purpose vs. "Utils" Dumping Ground — Page 13
    
-   **Interplay Between Coupling and Cohesion** _(Quick Self-Test)_ — Page 14
    

### 4. Flow of Control vs. Source Code Dependency

-   **The Two Arrows:** Runtime Control Flow vs. Compile-Time Dependencies — Page 15
    
-   **The Universal Law of Procedural Code** — Page 16
    
-   **The Procedural Call Tree & Upward Propagation of Details** — Page 17
    
-   **Real-World Impact:** Checkout $\rightarrow$ Stripe Dependency Chain — Page 18
    

### 5. The Three "Magic Words" of OO — An Honest Audit

-   **Encapsulation:** C vs. C++ Header Isolation Audit — Page 19
    
-   **Inheritance:** Diamond Problem, MRO, and Abstract Classes vs. Interfaces — Page 20
    
-   **Polymorphism (The Real Prize):**
    
    -   UNIX `copy` Program & Function Pointer Tables (vtable) — Page 21
        
    -   Static Typing vs. Dynamic Duck Typing — Page 22
        
-   **The Audit Scorecard & True Purpose of OO** — Page 23
    

### 6. Inversion of Control (IoC) — The Real Reason OO Exists

-   **Reversing Source Code Dependencies via Abstractions** — Page 24
    
-   **Hollywood Principle:** Control Shift from Details to Policy — Page 25
    
-   **Worked Examples:** Checkout $\rightarrow$ Stripe Inverted & Order Notifications — Page 26
    
-   **Untangling Concepts:** IoC vs. DIP vs. DI — Page 27
    

### 7. The SOLID Principles

-   **Single Responsibility Principle (SRP):** Actor-Based Changes & Refactoring — Page 28
    
-   **Open-Closed Principle (OCP):** Switch-Statement Disease & Abstractions — Page 29
    
-   **Liskov Substitution Principle (LSP):** Rectangle/Square & Bird/Penguin Violations — Page 30
    
-   **Interface Segregation (ISP) & Dependency Inversion (DIP)** — Page 31
