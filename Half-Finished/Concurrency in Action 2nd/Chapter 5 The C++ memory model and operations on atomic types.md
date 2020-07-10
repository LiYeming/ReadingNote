# Chapter 5 The C++ Memory Model and Operations on Atomic Types

C++ multithreading-aware memory model.

## Memory model basics
- Structual aspects
- Concurrency aspects

An object is stored in one or more memory locations.

- Every variable is an object, including those that are members of other objects.
- Every object occupies at least one memory location.
- Variables of fundamental types occupy exactly one memory location, whatever their size, even if they're adjacent or part of an array.
- Adjacent bit fields are part of the same memory location.

If two threads access the same memory location, and trying to modify the data, there's a potential for a race condition.

To avoid race condition, there must be some defined ordering.

- mutexes
- atomic operations.

If there is no enforced ordering, both access are not atomic.

Every object in C++ program has a modification order composed of all the writes to that object from all threads in the program.

Using atomic operations, compiler will provide sufficient synchronization to ensure that threads agree on the modification order of each variable.

All threads must agree on the modification orders of each individual object in a program, but no agreement on relative order.

## Atomic Operations and Types in C++

An atomic operation is an *indivisible operation*. For any thread in the system, it is either done or not done.

On the other hand, a non-atomic operation might be seen as half-done by  another thread.

The standard atomic types:

- All operations on such types are atomic.
- Use is_lock_free() to check if its implementation has used a lock.
  Also X::is_always_lock_free in c++17 and a set of Macros.
  - Prefer easier-to-get-right mutex based implementation than atomic operations.

Building block, and easy case: std::atomic_flag

- Guaranteed lock-free.

- clear(), test_and_set(), 

  - clear() is a store operation and can't have memory_order_acquire or memory_order_acq_rel semantics.

    test_and_set() is a read-modify-write operation and so can have any of the memory-ordering tags applied.

    With every atomic operation, the default for both is memory_order_seq_cst.

Storing a new value (or not) depending on the current value

- Building block of lock-free programming.

- `compare_exchange_weak()` can fail spuriously.

  On machines that lack a single compare-and-exchange instruction,  processor can't guarantee its atomicity, then would possibly fail.

  Typically used in a loop.

- Use `compare_exchange_strong()` when `expected` value is costly to store.
- Two memory ordering parameters, one for success , one for failure.

Pointer arithmetic

- two new atomic operations: `fetch_add()` and `fetch_sub()`. Also known as exchange-and-add.

Operations on standard atomic integral types

- `fetch_and()`, `fetch_or()`, `fetch_xor()`, same as `fetch_add()` and `fetch_sub()`.



The std::atomic<> primary class template

- In order to use `std::atomic<T>`, T must have trivial-copy-assignment operator.
  - no virtual functions, no virtual base class
  - compiler generated copy assignment operator.
  - trivial-copy-assignment requirement for all non-static member and base classes.
  - These requirements allows `memcpy()` or `memcmp()` like operation.
- if `sizeof(T) <= sizeof(void*)`, very likely to be lock-free.
  if `sizeof(T)  <= 2 * sizeof(void*)`, need DWCAS (double word compare and swap) instruction support.


## Synchronizing Operations and Enforcing Ordering

Memory model relations: *happens-before* and *synchronizes-with* relationship.

### The synchronizes-with relationship

The *synchronizes-with* relationship is something that you can get only between operations on atomic types.

> A write operation W on X 
>
> *synchronizes with* 
>
> A read operation on X, X changed by 
>
> - write op W 
> - Subsequent write op after W in the same thread
> - A sequence of atomic read-modify-write ops, by any thread, subsequent to W.

C++ memory model allows various ordering constraints to be applied to the operations on atomic types.

### The happens-before relationship

The *happens-before* and *strongly-happens-before* relationships are the building blocks of operation ordering in a program.

- Which operations sees the effects of which other operations.
- Single thread, sequenced before = happens before = strongly happens before
- Multiple thread case
  - if op A inter-thread-happens-before B, A happens before B.
  - if A synchronizes-with B, A inter-thread-happens-before B.
  - A ithb B, B ithb C -> A ithb C.
  - A sb B, B ithb C -> A ithb C.
- The difference is the`memory_order_consume` participate in inter-thread-happens-before but not `strongly-happens-before`.



### Memory ordering for atomic operations

Six memory orderings: 

- memory_order_relaxed
- memory_order_release
- memory_order_acquire
- memory_order_acq_rel
- memory_order_seq_cst (Default)
- memory_order_consume

Three models:

- *sequentially consistent* ordering
- *acquire-release* ordering
- *relaxed* ordering

Additional synchronization instruction are needed,

- `seq_cst` > `acq_rel` > `relaxed`.
- Multi-core processors needs more time.
  - x86, x86-64 don't require any additional instructions for acquire-release ordering beyond those necessary.
  - sequentially consistent load needs no special treatment, store needs some special treatment.

#### Sequentially consistent ordering

consistent with a simple sequential view of the world.

- All thread see the same order of ops.
- sc store synchronizes with a sc load. any sc ops after the load should  also appear after the store.
- On a weakly-ordered machine with many processors, it can impose a noticeable performance penalty.

#### Non-sequentially consistent memory ordering

There's no longer a single global order of events.

- *different threads can see different views of the same operations.*
- *threads don't have to agree on the order of events.*

Compiler and CPU can both reorder the instructions.

*In the absence of other ordering constraints, the only requirement is that all threads agree on the modification order of each individual variable.*

- Relaxed ordering
  - Operations on atomic types performed with relaxed ordering don’t participate in synchronizes-with relationships.
  - Operations on the same variable within a single thread
    still obey happens-before relationships. but almost no requirement on ordering relative to other threads.
  - Accesses to a single atomic variable from the same thread can't be reordered.
  - The modification order of each variable is the only thing shared between threads that are using `memory_order_relaxed`
  - relaxed operations on different variables can be freely reordered provided they obey any happens-before relationships they're bound by.

- Acquire-release ordering
  - A release operation synchronizes-with an acquire operation
    that reads the value written.
  - The value stored by a release operation must be seen by an acquire operation for either to have any effect.
  - acquire-release ordering can be used to synchronize data across several threads.

- Fences
  - Operations that enforce memory-ordering constraints without modifying any data and are typically combined with atomic operations that use the memory_order_relaxed ordering constraints.
  - Fence synchronization depends on the values read or written by operations before or after the fence.

### Ordering non-atomic operations

Ordering of non-atomic operations through the use of atomic operations is where the sequenced-before part of happens-before becomes so important.