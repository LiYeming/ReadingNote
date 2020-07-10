# Designing lock-based concurrent data structures

- What it means to design data structures for concurrency
- Guidelines for doing so
- Example implementations of data structures designed for concurrency

## What does it mean to design for concurrency?

*Thread safe*, each thread will see a self-consistent view of the data structure.

- No corrupted data
- Preserved invariants
- No race conditions

*Serialization*, threads take turns accessing the data protected by the
mutex.



*General idea*, the smaller the protected region, the fewer operations
are serialized, and the greater the potential for concurrency.

### Guidelines for designing data structures for concurrency

- Broken of invariants are only visible by one thread.
- Avoid race conditions by designing interface to be complete operation rather than operation steps.
- Pay attention to exception to ensure invariants are not broken.
- Minimize deadlock by  restricting the scope of locks and avoid nested locks.

Questions: Which functions are safe to call from other threads?

- Exclusive access for constructors and destructors.
- Assignment, swap, and copy construction, needs further consideration.

Central Questions: How can you minimize the amount of
serialization that must occur and enable the greatest amount of true concurrency?

The simplest thread-safe data structures typically use mutexes and locks to protect the data.

## Lock-based concurrency data structures

- Right mutex is locked when accessing data.
- Lock is held for the minimum amount of time.

### A thread-safe stack using locks

```c++
#define STD ::std::
template <class T> class threadsafe_stack {
private:
  mutable STD mutex m;
  STD stack<T> data;

public:
  threadsafe_stack() = default;

  explicit threadsafe_stack(const threadsafe_stack &other)
      : data([&other] {
          STD scoped_lock(other.m);
          return other.data;
        }()) {}

  threadsafe_stack &operator=(const threadsafe_stack &) = delete;
  void push(T newvalue) {
    STD scoped_lock lock(m);
    data.push(STD move(newvalue));
  }

  [[nodiscard]] STD shared_ptr<T> pop() {
    STD scoped_lock lock(m);
    if (data.empty())
      [[unlikely]] { throw STD empty_stack(); }
    else
      [[likely]] {
        STD shared_ptr result = make_shared(STD move(data.top()));
        data.pop();
        return result;
      }
  }

  bool empty() const {
    STD scoped_lock lock(m);
    return data.empty();
  }
};
```

- No thread can see a broken invariant.
- Potential race condition between empty() and pop().
- Exception:
  - Mutex related, no data changed, exception safe.
  - data.push() exception, stack<> guarantee.
  - empty_stack exception, no data changed, exception safe.
  - Creation of result, safety ensured by STL implementation of shared_ptr and no data change. data.pop() is noexcept.
  - empty() doesn't modify any data, thus exception-safe.
- Deadlocks:
  - User defined operator new.
  - Call user code while holding a lock.

Half-created object or Half-destructed object,

- Managed by user.

### A thread-safe queue using locks and condition variables

```c++
template <class T> class threadsafe_queue {
private:
  mutable STD mutex mut;
  STD queue<T> data_queue;
  STD condition_variable data_cond;

public:
  threadsafe_queue() = default;
  void push(T new_value) {
    STD scoped_lock lock(mut);
    data_queue.push(new_value);
    data_cond.notify_one();
  }
  void wait_and_pop(T &value) {
    STD unique_lock lock(mut);
    data_cond.wait(lock, [this] { return !data_queue.empty(); });
    value = STD move(data_queue.front());
    data_queue.pop();
  }
  STD shared_ptr<T> wait_and_pop() {
    STD unique_lock lock(mut);
    data_cond.wait(lock, [this] { return !data_queue.empty(); });
    STD shared_ptr<T> res(STD make_shared(std::move(data_queue.front())));
    data_queue.pop();
    return res;
  }
  bool try_pop(T &value) {
    STD scoped_lock lock(mut);
    if (data_queue.empty())
      [[unlikely]] { return false; }
    else
      [[likely]] {
        value = STD move(data_queue.front());
        data_queue.pop();
        return true;
      }
  }
  STD shared_ptr<T> try_pop() {
    STD scoped_lock lock(mut);
    if (data_queue.empty())
      [[unlikely]] { return STD shared_ptr<T>(); }
    else
      [[likely]] {
        STD shared_ptr<T> result(
            STD make_shared(std::move(data_queue.front())));
        data_queue.pop();
        return result;
      }
  }
  bool empty() const {
    STD scoped_lock lock(mut);
    return data_queue.empty();
  }
};
```

Same as the implementation above,

- data_cond.notify_one()
- wait_and_pop()
- Exception safety
  - only one thread awoken by the data_cond.notify_one().
  - throws an exception and none of the other threads will be woken.
    - Call notify_all().
    - notify_one in exception handler.
    - push pointers instead of values. delegate exception handling to push. and exclude resource allocation out of locks.

### A thread-safe queue using fine-grained locks and condition variables

Look inside the queue at its constituent parts and associate one mutex with each distinct data item.



## Designing more complex lock-based data structures

Design of a look-up table

### Writing a thread-safe lookup table using locks

std::map<> interface from concurrency perspective is its iterators.



Basic operations

- Add a new key/value pair.
- Change the value associated with some given key.
- Remove a key and its associated value.
- Obtain the value associated with a given key, if any.

#### Designing a map data structure for fine-grained locking.

















## Designing more complex lock-based data structures



### Writing a thread-safe list using locks





