[<~ Back to LLD](../lld.md)

### **Core Topics & Page Mappings** [PDF](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/VamshiOrganization/SystemDesign/main/notes/classes/lld/class-02/class-02-notes.pdf)

1.  **Single Responsibility + Separation of Concerns** — **Page 1**
    
    
    
    -   Layering responsibilities: `QuestionController` (HTTP), `QuestionService` (Business Rules), `QuestionRepository` (Data Access).
        
        
        
    -   The SRP isolation test (storage changes vs. email template changes).
        
        
        
2.  **Dependency Inversion (DIP) + Inversion of Control (IoC)** — **Page 1**
    
    
    
    -   Depending on abstractions (`QuestionRepository` interface) over concrete classes.
        
        
        
    -   Constructor injection & the Composition Root (`App.main()`).
        
        
        
3.  **Encapsulation, Tell-Don't-Ask, and the Rich Domain Model** — **Page 1**
    
    
    
    -   Co-locating behavior with data (e.g., `question.score()`).
        
        
        
    -   Protecting invariants vs. Anemic Domain Models (getter-farms leading to god-classes).
        
        
        
4.  **Law of Demeter (LoD) — Talk Only to Immediate Friends** — **Page 1**
    
    
    
    -   Avoiding dot-chain coupling across multiple objects (e.g., `question.authorCity()` vs. reaching through strangers).
        
        
        
    -   Distinction between object-reaching chains and fluent builders/streams.
        
        
        
5.  **Information Hiding** — **Page 1**
    
    
    
    -   Exposing intent over implementation mechanisms (e.g., `ConcurrentHashMap` and `AtomicLong` hidden inside `InMemoryQuestionRepository`).
        
        
        
6.  **Immutability + Defensive Copying** — **Page 1**
    
    
    
    -   Preventing external reference corruption using defensive copies (`List.copyOf(tags)`) inside Java records.
        
        
        
    -   Benefits: Thread safety without locks, safe caching, and explicit state evolution.
        
        
        
7.  **Value Objects vs. Entities** — **Page 1**
    
    
    
    -   **Entities:** Identified by ID (e.g., `Question`).
        
        
        
    -   **Value Objects:** Interchangeable, equality by field values (e.g., `QuestionFilter`, `Cursor`, Java records).
        
        
        
8.  **The DTO Boundary + Transformer Pattern** — **Page 1**
    
    
    
    -   Preventing entities from crossing the HTTP boundary via static factory transformers (`QuestionSummaryResponse.from(q)`).
        
        
        
9.  **API Evolution + Postel's Law** — **Page 1**
    
    
    
    -   _"Be conservative in what you send; be liberal in what you accept"_.
        
        
        
    -   Evolution mechanisms: Opaque Base64 cursors, ignoring unknown JSON fields, computed fields.
        
        
        
10.  **HTTP Semantics — Safe, Idempotent, PUT vs. PATCH** — **Page 1**
    
    
    
    -   **Safe:** `GET`, `HEAD`, `OPTIONS`
        
        
        
    -   **Idempotent:** `GET`, `PUT`, `DELETE`
        
        
        
    -   **Non-Idempotent:** `POST`
        
        
        
    -   Surviving retries in distributed systems (PUT vs. PATCH).
        
        
        
11.  **Cursor Pagination Mechanics** — **Page 1**
    
    
    
    -   Offset pagination limitations (drift & high DB read costs) vs. Cursor seek pagination.
        
        
        
    -   Opaque Base64 tokens and deterministic sorting using unique tie-breakers (`(sortValue, id)`).
        
        
        

### **Summary: The 5 Mental Model Shift Principles** — **Page 1**



-   **Law of Demeter**
    
    
    
-   **Tell, Don't Ask**
    
    
    
-   **DIP + IoC**
    
    
    
-   **Rich Domain Model**
    
    
    
-   **Postel's Law**
    
    
