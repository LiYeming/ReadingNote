# Designning lock-free concurerent data structures

- Using locks
  - Use one single mutex to protect entire data structure.
  - Use more than one mutex to protect various smaller parts of the data structure, for greater concurrency.
- Lock-free
  - Memory-order properties of atomic operations.

## Definition and Consequences

Algorithms and data structures that use mutexes, condition variables, and futures to synchronize the data are called *blocking* data structures and algorithms.



### Types of nonblocking data structures

*non-blocking*

- Obstruction-free, if all other threads are paused, then any given thread will complete its operation in a bounded number of steps.
- Lock-free, if multiple threads are operating on a data structure, then after a bounded number of steps one of them will complete its operation.
- Wait-free, every thread operating on a data structure will complete its operation in a bounded number of steps, even if other threads are also operating on the data structure.

### Lock-free data structures

### Wait-free data structures

Lock-free data structure with additional properties

- Every thread accessing the data structure can complete its operation within a bounded number of steps, regardless the behavior of other threads.
- Writing wait-free data structure correctly is extreme hard.

### Pros and cons of lock-free data structures

Pros:

- Enable maximum concurrency.
- Robustness, consider a thread dies while holding a lock.

Cons:

- Hard to hold invariants.
- Needs to pay attention to the ordering constraints you impose on the operations.
- May **decrease** overall performance.
  - Atomic operations are much slower than non-atomic operations
  - Hardware must synchronize data between threads that access the same atomic variables.

## Examples of lock-free data structures

Managing memory in lock-free data structures

- Problem: when freeing a node, can't be sure that other threads are not using it.
- Need to write a special-purpose garbage collector for nodes.
- First idea, thread count
  - if only one thread is popping, it would be safe
  - if not , store these nodes in a list.
  - no deletion in high load situation.
- Second idea, hazard pointers
  - Set a flag(hazard pointer, a pointer to the object) when thread access some object. clear the flag after finishing access this object.
- Third idea, reference counting
  - Storing a count of the number of threads accessing each node.

Finally, relax the memory ordering

- First find the operations and identify the required relationships between them.
  - Look at the situation from the pointer of view of threads in several different scenarios.