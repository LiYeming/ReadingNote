## Chapter 4 Synchronizing concurrent operations

*Conditional Variables*,  *Futures*, *Latches*, *Barriers* to synchronizing multiple threads.



Thread waiting for another thread,

- Repetitive checking, wasteful
- Sleep for short periods, std::this_thread::sleep_for() function, a little better.
- Conditional Variable, event based wakeup call, preferred.
  - std::conditional_variable and std::conditional_variable_any, the former works with std::mutex only , while the latter works something like mutex, at the cost of additional resources.
  - Fundamentally, std::conditional_variable::wait is an optimization over a busy-wait.