# Reactive Design Pattern

## Testing Reactive Applications

### How to Test

Vocabulary:
- Failure is an unexpected event within a service that prevents it from continuing to function normally
- Error is an expected and coded-for condition.

- Unit tests
- Component tests
- String tests
- Integration tests
- User-acceptance tests
- Black-box vs. White-box tests

### Testing asynchronously
Pervasive use of asynchronous message passing requires a different way of formulating test cases.

### Testing non-deterministic systems
Due to inherent concurrency and unreliable communication, the determinism cannot always be achieved.

### Testing elasticity

### Testing resilience
Resilience is achieved by replication, containment, isolation, and delegation.
- break down the failure case from micro to macro level.

### Testing responsiveness
Forget qualitative test such as average, use percentiles.

### Summary
- Testing must begin at the outset of the project and continue throughout every phase of its life-cycle.
- Testing must be functional and non-functional to prove that an application is Reactive.
- You must write tests from the inside of the application outward to cover all interactions and verify correctness.
- Elasticity is tested externally, whereas resilience is tested in both infrastructure and internal components of the application.

## Fault Tolerance and Recovery Patterns

How to incorporate the possibility of failure into the design of application.

- Simple Component pattern
- Error Kernel pattern
- Let-It-Crash pattern
- Circuit Breaker pattern

### Simple Component Pattern

*A component shall do only one thing, but do it in full*



### Error Kernel Pattern

### Let-It-Crash Pattern

### Circuit Breaker Pattern



### Summary

Implementation of resilient systems:

- Simple components that obey the single responsibility principle
- Application of hierarchical failure handling in practice while implementing the Error Kernel pattern
- Implications of relying on components restarts to recover from failure
- Decouple components from each other using the Circuit Breaker pattern for either side's protection.







## Replication Patterns

## Resource-management Patterns

## Message Flow Patterns

## Flow Control Patterns

## State Management and Persistence Patterns







