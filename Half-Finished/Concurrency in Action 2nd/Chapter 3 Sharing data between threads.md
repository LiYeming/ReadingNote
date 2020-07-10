## Chapter 3 Sharing data between threads

Sharing data between threads could cause serious bug:

- If shared data could be modified by threads, invariants may be broken.
  - A race condition is anything where the outcome depends on the relative ordering of execution of operations on two or more threads. Time dependent, thus nasty for debugging.
- Using a protection mechanism to ensure only the thread performing modification can see the broken of invariants.
- Another way of dealing with race conditions is to handle the updates to the data structure as a transaction. The required series of data modifications and reads is stored in a transaction log and then committed in a single step.

Mutex ensures no threads can visit a locked mutex.

- lock the mutex, do something, unlock the mutex.
- To use mutex properly, structure code to protect the right data and avoid race conditions inherent in interfaces, and avoid deadlocks.
- Create a mutex with std::mutex, lock it with lock() and unlock it with unlock().
  - std::lock_guard implements the RAII idiom for mutexes, and is grouping with protected data.
  - But the pointer mechanic may bypass the protection of mutexes, essentially we should structure the code to achieve mutually exclusive.
    - Rule of thumb: Don't pass pointers and references to protected data outside the scope of the lock.

Mutex is not enough for prevention of race condition,

- Interface is the problem, calling one method doesn't prevent other threads to call another method. Thus a classical race condition occurs.
  - Solution: Change the interface. Lock larger area, but eliminate the gain from concurrency.

Multiple semaphore(mutex) may result in a deadlock, each is waiting for the other to unlock.

- Solution, always lock in the same order and unlock in the reverse order.

  - Swap function with a fixed locking routine, lock A, then B. Consider swap(A,B) and swap(B,A) happening concurrently, there would be a deadlock for both threads.
  - std::lock for locking two or more mutex at the same time, all or nothing. std::adopt_lock for a std::lock_guard to control a locked mutex.
  - In C++17, we can use std::scoped_lock<> to equivalently lock multiple mutexes.

- Deadlocks can happen without locks, for example, two threads waiting for each other to join().

  - Rule of Thumb: Don't wait for another thread if there's a chance it's waiting for you.
  - Rule no. 1, Avoid nested locks.
  - Rule no. 2, Avoid calling unsafe code while holding a lock.
  - Rule no. 3, Acquire locks in a fixed order, if you must use multiple locks.
  - Rule no. 4, Use a lock hierarchy, divide application into layers and identify all mutexes that may be locked in any given layer. It is not possible to lock a mutex, if already holding a lock from lower layer.
    - Using thread_local value representing the hierarchy value for the current thread. The value is accessible to all mutex instances, but has a different value on each thread.
  - Extending these guideline beyond locks, using these guideline for other Deadlockers.

- More flexibility

  - std::unique_lock, allowing an std::unique_lock instance not own the mutex, but with overhead: This information has to be stored and updated, thus slower and more memory consuming.
  - When mutex is locked according to some program state, unique_lock may be useful.
    - A Gateway class for example.

- Appropriate granularity

  - If multiple threads are waiting for the same resource, then if any thread holds the lock for longer than necessary, it will increase the total time spent waiting.
    - Where possible, lock a mutex only while accessing the shared data.
    - Try to do any processing of the data outside of the lock. 
      - Time consuming process like file I/O.
    - In general, a lock should be held for only the minimum possible time needed to perform the required operations.

- Double-checked locking pattern:

  - ```c++
    void foo()
    {
        if (!resource_ptr){
            std::scoped_lock<std::mutex>(resource_mutex);
            if (!resource_ptr) {
                resource_ptr.reset(new resource);
            }
        }
        resource_ptr -> do_something();
    }
    ```

  - do_something() may run on half-constructed resource.

    - Use std::call_once(), and std::once_flag to  lower overhead of mutex explicitly.
    - Initialization of a local static is thread-safe in C++

- Updating data structure may use *reader-writer* mutex.

  - std::shared_mutex and std::shared_timed_mutex.
  - Readers use std::shared_lock to obtain shared access, Writers use std::lock_guard for exclusive access. The exclusive access is blocked until all shared_locks are unlocked.

- std::recursive_mutex is essentially std::mutex, multiple locks from a single instance in a single thread. call lock() three times, unlock will call three times.

  - a class in which all methods lock the same mutex, while some method reuse some other methods. need a recursive mutex in this case.
  - Bad practice of using it, using methods when locked means invariants cannot preserve, may result in bugs or undefined behavior.



