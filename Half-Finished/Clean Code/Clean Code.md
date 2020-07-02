# Clean Code

- Meaningful names
  - Use intention-revealing names
  - Avoid disinformation
  - Make meaningful distinctions
  - Use pronounceable names
  - Use searchable names
  - Avoid encodings
  - Avoid mental mapping, clarity is king
  - Class Names should hae noun or noun phrase names
  - Method names should have verb or verb phrase names
  - Pick one word per concept
  - Don't pun
  - Use solution domain names for programmers
  - Use problem domain names for users
  - Add meaningful context
  - Don't add gratuitous context
- Functions
  - Functions should be small, very small.
    - 3 - 4 lines of length would be sufficient.
  - Do one thing
    - Functions should do one thing
    - They should do it well
    - They should do it only
  - One level of abstraction per function
  - Switch statements can be buried into an abstract factory.
  - Use descriptive names
    - A long descriptive name is better than a short
      enigmatic name
    - A long descriptive name is better than a long descriptive comment
  - Number of function argument should be less than three, generally.
    - Wrap more than three arguments into a class.
  - Have no side effects
    - In general output arguments should be avoided.
  - Command query Separation
  - Prefer exceptions to returning error codes
    - Extract try/catch blocks, put try block and catch block to different functions.
    - Error Handling is one thing, a function handles error should do nothing else.
  - Don't repeat yourself. (DRY principle)
  - Structured programming, keep functions small can ensure this princple.
  - Master programmers think of systems as stories to be told rather than programs to be written. **The real goal is to tell the story of the system.**
- Objects(private data and public methods)and Data Structures(public data, no methods)
  - Hiding implementation is about abstractions! Express data in abstract terms.
  - Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. 
    OO code, on the other hand, makes it easy to add
    new classes without changing existing functions.
  - The Law of Demeter
    - A module should not know about the innards of the objects it manipulates.
  - Avoid creating half object and half data structure hybrids.
  - Data Transfer Objects (DTO, data structure with public variables and no functions.)
- Error Handling
  - Error handling is important, but if it obscures logic, it’s wrong.
  - Use Exceptions Rather Than Return Codes.
  - Don't return null
- Boundaries
  - Using third-party code
    - provider of interface and user of interface, portability v.s. specialization.

---

Emergence

- Getting Clean via Emergent Design
  - Runs all the tests, writing tests leads to better designs.
  - Contains no duplication
  - Expresses the intent of the programmer
  - Minimizes the number of classes and methods



---

Concurrency

- Why concurrency?
  - Decouple what gets done from when it gets done.
  - Myths and Misconceptions
    - Concurrency incurs some overhead.
    - Correct concurrency is complex.
    - Concurrency bugs aren't usually repeatable.
    - Concurrency often requires a fundamental change in design strategy.
- Concurrency defense principles
  - Single Responsibility Principle
    - Concurrency-related code has its own life cycle of development, change and tuning.
    - Concurrency-related code has its own challenges, which are different from and often more difficult than non-concurrency-related code.
    - The number of ways in which miswritten concurrency-based code can fail makes it challenging enough without the added burden of surrounding application code.
    - **Keep your concurrency-related code separate from other code.**
  - Corollary: Limit the Scope of Data
    - Take data encapsulation to heart; severely limit the access of any data that may be shared.
  - Corollary: Use Copies of Data
    - A good way to avoid shared data is to avoid sharing the data in the first place.
  - Corollary: Threads should be as Independent as Possible
    - Attempt to partition data into independent subsets that can be operated on by independent threads, possibly in different processors.
- Know your library
  - Building blocks to support advanced concurrency design
    - ReentrantLock, a lock that can be acquried in one method and released in another
    - Semaphore, a lock with a count
    - CountDownLatch, a lock that waits for a number of events before releasing all threads waiting on it.
- Execution Models
  - Definitions:
    - Bound Resources, resources with a fixed size.
    - Mutual Exclusion, only one thread can access shared data at a time.
    - Starvation, One or a group of threads is prohibited from proceeding for an excessively long time or forever.
    - Deadlock, Two or more threads waiting for each other to finish.
    - Livelock, each thread trying to do work but finding another ``in the way''.
  - Most Concurrent Problems will be some variation of these three problems.
    - Producer-Consumer
    - Reader-Writer
    - Dining Philosophers
  - Learn these basic algorithms and understand their solutions
- Beware dependencies between synchronized methods
  - Avoid using more than one method on a shared object.
    - Client-based locking
    - Server-based locking
    - Adapted Server
- Keep Synchronized Sections Small
  - Keep your synchronized sections as small as possible
- Writing Correct Shut-Down Code is Hard
  - Think about shut-down early and get it working early. It's going to take longer than you expect. Review existing algorithms because this is probably harder than you think.
- Testing Threaded Code
  - Write tests that have the potential to expose problems and then run them frequently, with different programatic configurations and system configurations and load. If tests ever fail, track down the failure. Don't ignore a failure just because the tests pass on a subsequent run.



