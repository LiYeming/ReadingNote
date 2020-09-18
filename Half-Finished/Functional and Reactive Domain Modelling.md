# Functional and Reactive Domain Modelling

## Preface 

Tools domain-driven design, functional programming and reactive principles among the most useful tools we've developed.

- Domain complexity, 
  DDD makes it easier to create extensible domain models that map to the real world, while allowing for continuous change.

- Solution complexity,

  Functional programming helps us with reasonability and composability.

- System complexity,
  Reactive principles can help us manage the increasingly complex world of multi-core processors, cloud computing, mobile devices and the IoT from day one.

### Roadmap

- Chapter 1, overall discussion of the general content.
- Chapter 2, benefits of using Scala as the implementation language for functional and reactive domain modeling.
- Chapter 3, discussion of algebraic API design.
- Chapter 4, functional design patterns.
- Chapter 5, modularizing your domain models.
- Chapter 6, reactive domain models.
- Chapter 7, reactive streams.
- Chapter 8, domain model persistence.
- Chapter 9, testing domain models.
- Chapter 10, conclusion.

---

## Chapter 1. Functional domain modeling: an introduction

---

- Domain models and domain-driven design.
- Benefits of functional and pure domain models.
- Reactive modeling for increased responsiveness.
- How functional meets reactive.

Domain model and DDD

- Benefits from functional programming 
  - Purity
  - Referential transparency
  - Ability of reasoning
  - Compositionality
- Benefits from reactive programming
  - Resiliency
  - Responsiveness
  - Scalability
  - Event orientation

### What is a domain model?

A domain model is a blueprint of the relationships between the various entities of the problem domain and sketches out other important details, such as the following:

- *Objects that belong to the domain.*
  - banks, accounts and transactions, etc.
- *Behaviors that those objects demonstrate in interacting among themselves.*
  - issue a statement, debit an account, etc.
- *The language that the domain speaks.*
  - terms such as debit, credit, portfolio, etc.
- *The context within which the model operates.*
  - a set of assumptions and constraints that are relevant to the problem domain and are automatically applicable for the software model that you develop.

Essential(inherent) complexity and Incidental(implementation) complexity

- Effective model implementation is reducing incidental complexity.

Modularization, decompose the software into multiple smaller components each of which functions within its own context and assumptions.

- Self-contained components in functionality.
- Interact with other components only through explicitly defined contracts.

### Introducing DDD

Understanding the domain and abstracting the central characteristics in the form of a model is known as *domain-driven design*.

#### The bounded context

Any domain model of non-trivial complexity is really a collection of *bounded context*, which are smaller models, each with its own data and domain vocabulary.

Communication between bounded contexts

- Explicitly specified sets of services or interfaces.
- The basic idea is to keep these interactions to the bare minimum so that each bounded context is cohesive enough within itself and yet loosely coupled with other bounded contexts.

#### The domain model elements

*Entities* and *Value objects*

- Entities has an *identity* that can't change, a value object has a value that can't change.
- Entities are semantically mutable, while a value object is semantically immutable.
- Difference between an entity and a value object is one of the most fundamental concepts in domain modeling.

The heart of any domain model is the set of behaviors or interactions between the various domain elements.

- In DDD, you model the entire set of behaviors as one ore more *services*.
- In a *service*, multiple domain entities interact according to specific business rules and deliver a specific functionality in the system.
- A *service* is a set of functions acting on a related set of domain entities and value objects.

![image-20200911102632763](/home/liyeming/.config/Typora/typora-user-images/image-20200911102632763.png)

![image-20200908122144867](/home/liyeming/.config/Typora/typora-user-images/image-20200908122144867.png)

#### Life-cycle of a domain object

Every object (entity or value object) that you have in any model must have a definite life-cycle pattern.

- Creation, 
  How the object is created within the system.
  
- Participation in behaviors, 
  How the object is represented in memory when it interacts within the system.
- Persistence, 
  How the object is maintained in the persistent form.

**How to handle these three life-cycle events?**

**Factories**

Use a factory to abstract the process of creation. After all, a factory provides a service of creation and initialization.

**Aggregates**

An *aggregate* can consist of one or more entities and value objects.

- One way the entire graph of objects is visualized is to think of it as forming a *consistency boundary* within itself.
- An *aggregate* within a *bounded context* is also often looked at as a transaction boundary in the model.

Aggregates forms a consistent boundary within itself. created by factories and represent the underlying entities in memory during the active phase of the objects' life-cycle.

One of the entities within the aggregate forms the *aggregate root*. The aggregate root has two objectives to enforce:

- Ensure the consistency boundary of business rules and transactions within the aggregate.
- Prevent the implementation of the aggregate from leaking out to its clients, acting as a facade for all the operations that the aggregate supports.

Entity should be updateable, but it is not mutable in place. New entities are created instead.

**Repositories**

A *repository* gives you this interface for parking an aggregate in a persistent form so that you can fetch it back to an in-memory entity representation when you need it.

- Relational database management system
  - Persistent model of the aggregate may be entirely different from the in-memory aggregate representation.
- It's the responsibility of the repository to provide the interface for manipulating entities from the persistent storage without exposing the underlying relational data model.

#### The Ubiquitous Language

*Vocabulary*, names of participating objects and the behaviors that are executed as part of the use cases.

With entities, value objects and services that forms the model, all these elements need to interact with each other to implement the various behaviors that the business executes.

- Function body is minimal and doesn't contain any irrelevant details.
- Use terms from the domain, so a non-programmer can understand as well.
- Exceptional paths are completely encapsulated within the abstractions that you use for implementation.

*ubiquitous language* uses the domain vocabulary in your model and make the terms interact in such a way that it resembles the language that the domain speaks.

- API must be expressive so that a person who's an expert in the domain can understand the context by looking at the API only. This is called *Domain-specific Language.*

### Thinking functionally

Mutable states, although appears to be a convincing way of modeling the world, create more problems than solutions.

The mainstream OOP encourages functions to be encapsulated under the same abstraction as the state. 

General principles you need to follow when designing functional domain models: 

- Model the immutable state in an *algebraic data type.*
- Model behaviors as functions in modules, where a module represents a coarse unit of business functionality
- Keep in mind that behaviors in modules operate on types that the *ADTs* represent.

composition of functions,

$f(x), g(y) -> g(f(x))$

Exceptions are considered impure.

![image-20200908132848672](/home/liyeming/.config/Typora/typora-user-images/image-20200908132848672.png)

### Managing side effects

Impure functions has to deal with external effects that belong to the outside world, which are known as side effects.

- I/O exception.

The best way to handle this is to decouple the side effects as far as possible from the pure domain logic.

![image-20200911115221865](/home/liyeming/.config/Typora/typora-user-images/image-20200911115221865.png)

### Virtues of pure model elements

*referentially transparent* expressions, can be replaced with its corresponding value without changing the program's behavior.

Three pillars of functional programming

![image-20200908134210192](/home/liyeming/.config/Typora/typora-user-images/image-20200908134210192.png)

### Reactive domain models

*latency*, the time period that elapses between the request that you make and the response that you get back from the server.

Reducing latency is key to reactive model. How to address failures?

- Design your system around failures.

![image-20200908134705124](/home/liyeming/.config/Typora/typora-user-images/image-20200908134705124.png)

The key that makes a model reactive is it being responsive.

![image-20200908134828977](/home/liyeming/.config/Typora/typora-user-images/image-20200908134828977.png)

Failure can occur that are completely beyond control, no matter how well exceptions are handled or how robustly you think you've managed exceptions.

Key lesson from reactive model is to design around failures and increase the overall resiliency of the model.

- Failures are handled not only from *within*, but also from other *external* sources.

![image-20200911122212772](/home/liyeming/.config/Typora/typora-user-images/image-20200911122212772.png)

Accept failures are certain to occur and implement strategies to handle them explicitly as they occur in various parts of your system.

- Have a separate module that handles failures. All failures are delegated to this module, which takes responsibility for handling them based on user-defined strategies.
- Centralized handlers, one single module for failure handling in the whole of your application.

### Event-driven programming

- Sequential execution, the latency of the total computation is high.
- Parallel execution, keeping the main thread in the role of a coordinator.
  - Non-blocking, increase in responsiveness.
  - Events are implicitly sent to the calling thread by the Future abstraction.

Event-driven programming models make events an important architectural element. Events trigger domain logic and take part in various interactions within the domain model.

Domain events

- Uniquely identifiable as a type
- Self-contained as a behavior
- Observable by consumers
- Time relevant

### Functional meets reactive

Any application needs to be responsive.

- Reactive models deliver responsiveness to failures, varying load, and concurrency.
  - Responsive to failures, resiliency built into the architecture by proper modularization of the model and ensuring that failure in one component doesn't bring down the entire system.
    - Failures need to be handled separately instead of being coupled with the domain logic.
  - Event driven, the main thread of execution is never blocked.
    - Various event handlers can run independently and work on executing domain behaviors.
  - Functional programming ensures Minimal sharing of state.

### Summary

- Avoid shared mutable state within your model
- Referential transparency
- Organic growth
- Focus on the core domain
- Functional makes reactive easier
- Design for failure
- Event-based modeling complements the functional model.



## Functional Patterns for Domain Models

This chapter covers

- Understanding functional design patterns and how they differ from OO design patterns
- Algebra as patterns
- Using monoids, the ubiquitous design pattern in functional progamming
- Working with patterns for programming with effects--functors, applicatives, and monads
- Two use cases from the field that use types, algebra and patterns to build domain models with better abstraction.

### Patterns--the confluence of algebra, functions, and types

![image-20200914204454113](/home/liyeming/.config/Typora/typora-user-images/image-20200914204454113.png)

Each functional pattern has two distinct parts:

- *Algebra*, Complete generic and reusable code which is invariant across all contexts where you use the pattern.
- *Interpreter*, A context-specific implementation that varies across all instances where you apply the pattern.

#### Functors -- The pattern to build on

#### Monadic effects -- a variant on the applicative pattern







### Summary

- *What are functional design patterns?* 

  Design patterns as viewed through the lens of functional programming are different from those in OOP. 
  In FP, the patterns are the algebra of abstractions, for which you can write specific interpreters depending on your context.

- *The ubiquitous monoid*

  Monoid is a design pattern that appears often in domain models. Monoids can make your model generic an help you abstract over the operations of your domain.

- *Pattern for effectful programming*

  The three most commonly used patterns in typed functional programming are functors, applicatives and monads.

- *Pattern goodness*

  Domain model should be made compositional, parametric, and reusable.

