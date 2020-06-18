## Chapter 4 Synchronizing concurrent operations

*Conditional Variables*,  *Futures*, *Latches*, *Barriers* to synchronizing multiple threads.



Thread waiting for another thread,

- Repetitive checking, wasteful

- Sleep for short periods, std::this_thread::sleep_for() function, a little better.

- Conditional Variable, event based wakeup call, preferred.
  - std::conditional_variable and std::conditional_variable_any, the former works with std::mutex only , while the latter works something like mutex, at the cost of additional resources.
  - Fundamentally, std::conditional_variable::wait is an optimization over a busy-wait.
  
- C++ STL models *future*, if a thread needs to wait for a specific one-off event, it somehow obtains a future representing that event.

  - Two kinds of futures, unique futures (std::future<>)and shared futures(std::shared_future<>). unique or shared access of the data associated with the event.

- Use std::thread to start a new thread, but how to get the result back?

  - Use std::async to start an asynchronous task for which you don't need the result now.
    Returns a std::future object, which eventually holds the result.
    Call get() on the future to get the result, and blocks threads until the future is ready.
  - Up to the implementation whether std::async starts a new thread or whether the task runs synchronously when the future is waited for.

- std::packaged_task<> ties a future to a callable object.

  - Invoke the underlying callable object and makes the future ready with return value stored.
  - Basically a wrapper of tasks can be used to construct a thread pool or task management schemes.

- std::promise<T> provides a means of setting a value of type T that can later be read through an associated std::future<T> object.

  - Futures and promises originated in functional programming and related paradigms to decouple a value (a future) from how it was computed (a promise), allowing the computation to be done more flexibly.
  - get_future() member function to get the std::future.
  - set_value() member function to set the value and get the future ready.
  - if std::promise is destroyed before a value is set, an exception is stored instead.

- Exceptions

  - Exception is stored in the future in place of a stored value. future is ready and a call to get() rethrows that stored exception.
  - Also, when std::packaged_task or std::promise is destroyed before setting a value, an exception is set.

- Waiting with a time limit

  - duration-based timeout (for a fixed-period of time) or absolute timeout (until an absolute time).
  - a clock (std::chrono::system_clock, std::chrono::steady_clock, std::chrono::high_resolution_clock)
    - a time now()
    - a type of the value
    - the tick period
    - whether or not the clock ticks at a uniform rate

- Focus of operations, forget mechanics.

  - Simplify code by adopting a more *functional approach* to programming concurrency.

    Rather than shared_data paradigm, each task can be provided with the data it needs, and the result can be disseminated to any other threads that need it through the use of futures.

  - The effect of functions are entirely limited to the return value. 

    - No shared data, no race conditions thus no mutexes.
    - Easier to distinct dangerous (impure) and safe (pure) functions.
    - *future* can be passed around between threads to allow the result of one computation to depend on the result of another, *without any explicit access to shared data*.

  - Alterantive paradigms exists, like CSP(Communicating Sequential Processes), where threads are conceptually entirely separate, with no shared data but with communication channels that allow messages to be passed between them.

    - no shared data, each thread is effectively a state machine. receive message to change state, and send message to other threads.
    - Threads share the same virtual memory space, thus needs discipline.
    - Use member function pointers to indicate state and state transition for each actor. No need to concern concurrency issues.
    - This style of programming can greatly simplify the task of designing a concurrent system.





