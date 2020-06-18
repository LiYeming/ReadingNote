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

An atomic operation is an indivisible operation. For any thread in the system, it is either done or not done.

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


## Synchronizing Operations and Enforcing Ordering

```c++
std::vector<int> data;
std::atomic<bool> data_ready(false);
void reader_thread() {
  while (!data_ready.load()) {
    std::this_thread::sleep(std::chrono::milliseconds(1));
  }
  std::cout << "The answer = " << data[0] << '\n';
}
void writer_thread() {
  data.push_back(42);
  data_ready = true;
}
```

data.push_back(42) before data_ready = true, before data_read.load() return true, before data[0].

- synchronizes provided by atomic<bool> data_ready.

the atomic operations also have other options for the ordering requirements.

### The synchronizes-with relationship

The synchronizes-with relationship is something that you can get only between operations on atomic types.

if thread A stores a value and thread B reads that value, there’s a synchronizes-with relationship between the store in thread A and the load in thread B.

C++ memory model allows various ordering constraints to be applied to the operations on atomic types.

### The happens-before relationship

The *happens-before* and *strongly-happens-before* relationships are the building blocks of operation ordering in a program.

If operation(A) occurs in a statement prior to another (B) in the source code, then A happens before B, and A strongly happens before B.

The new part is the interaction between threads: if operation A on one
thread inter-thread happens before operation B on another thread, then A happens before B.

If operation A is sequenced before operation B, and operation B interthread happens before operation C, then A inter-thread happens before C.

### Memory ordering for atomic operations

There are six memory ordering options that can be applied to operations on atomic types:

- Memory_order_relaxed
- Memory_order_consume
- Memory_order_acquire
- Memory_order_release
- Memory_order_acq_rel
- Memory_order_seq_cst

Three models are represented:

- Sequentially consistent ordering
- Acquire-release ordering
- Relaxed ordering

Consequences of each choice for operation ordering and synchronizes-with.

#### Sequentially consistent ordering

The default ordering, the behavior of the program is consistent with a simple sequential view of the world.

From the point of view of synchronization, an ordering constraint on the operation of two threads.

Straight-forward and Intuitive ordering, but it's also the most expensive memory ordering because it requires global synchronization between all threads.

#### NON - SEQUENTIALLY CONSISTENT MEMORY ORDERINGS

there's no longer a single global order of events. **Threads don't have to agree on the order of events**.

- Compiler can reorder the instructions
- Threads running the same bit of code can disagree on the order of events
- different CPU caches and internal buffers can hold different values for the same memory.

In the absence of other ordering constraints, the only requirement is that all threads agree on the modification order of each individual variable.

#### Relaxed atomic operations, just ignore it

The `memory_order_relaxed` arguments above mean “ensure these 

#### Acquire Release Ordering

Under this ordering mode, atomic loads are acquire operations, atomic stores are release operations. atomic read-modify-write operations are both.

A release operation synchronizes-with an acquire operation
that reads the value written.

Different thread can see different orderings, but these orderings are restricted.