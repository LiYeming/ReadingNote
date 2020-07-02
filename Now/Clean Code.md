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
    - 

